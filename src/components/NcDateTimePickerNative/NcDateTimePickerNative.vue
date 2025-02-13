<!--
  - @copyright Copyright (c) 2022 Julia Kirschenheuter <julia.kirschenheuter@nextcloud.com>
  -
  - @author Julia Kirschenheuter <julia.kirschenheuter@nextcloud.com>
  -
  - @license GNU AGPL version 3 or any later version
  -
  - This program is free software: you can redistribute it and/or modify
  - it under the terms of the GNU Affero General Public License as
  - published by the Free Software Foundation, either version 3 of the
  - License, or (at your option) any later version.
  -
  - This program is distributed in the hope that it will be useful,
  - but WITHOUT ANY WARRANTY; without even the implied warranty of
  - MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
  - GNU Affero General Public License for more details.
  -
  - You should have received a copy of the GNU Affero General Public License
  - along with this program. If not, see <http://www.gnu.org/licenses/>.
  -
  -->

<docs>

### General description

This components provides a wrapper around the native browser datetime picker. <br>
This is an input with some type of date e.g. https://html.spec.whatwg.org/multipage/input.html#local-date-and-time-state-(type=datetime-local). <br>
All available types are: 'date', 'datetime-local', 'month', 'time' and 'week', please check them here: https://html.spec.whatwg.org/multipage/input.html#  <br>

### Examples

#### Usage: type='datetime-local'
```vue
<template>
	<span>
		<NcDateTimePickerNative
			v-model="value"
			:id="id"
			:label="label"
			type="datetime-local" />
	</span>
</template>
<script>
	export default {
		data() {
			return {
				value: new Date(),
				id: 'date-time-picker',
				label: 'please select a new date',
			}
		},
	}
</script>
```

#### Usage: type='datetime-local' with min date and max date
```vue
<template>
	<span>
		<NcDateTimePickerNative
			v-model="value"
			:id="id"
			:min="yesterdayDate"
			:max="someDate"
			:label="label"
			type="datetime-local" />
	</span>
</template>

<script>
	export default {
		data() {
			return {
				value: new Date(),
				id: 'date-time-picker',
				label: 'please select a new date',
				yesterdayDate: new Date(new Date().setDate(new Date().getDate() - 1)),
				someDate: new Date(new Date().setDate(new Date().getDate() + 7)),
			}
		},
	}
</script>
```

#### Usage: type='week'
```vue
<template>
	<span>
		<NcDateTimePickerNative
			v-model="value"
			:id="id"
			:label="label"
			type="week" />
	</span>
</template>

<script>
	export default {
		data() {
			return {
				value: new Date(),
				id: 'date-time-picker',
				label: 'please select a new date',
			}
		},
	}
</script>
```

#### Usage: type='month'
```vue
<template>
	<span>
		<NcDateTimePickerNative
			v-model="value"
			:id="id"
			:label="label"
			type="month" />
	</span>
</template>

<script>
	export default {
		data() {
			return {
				value: new Date(),
				id: 'date-time-picker',
				label: 'please select a new date',
			}
		},
	}
</script>
```

</docs>

<template>
	<div class="native-datetime-picker">
		<label :class="{ 'hidden-visually': hideLabel }" :for="id">{{ label }}</label>
		<input :id="id"
			class="native-datetime-picker--input"
			:type="type"
			:value="formattedValue"
			:min="formattedMin"
			:max="formattedMax"
			v-bind="$attrs"
			v-on="listeners">
	</div>
</template>

<script>

const inputDateTypes = ['date', 'datetime-local', 'month', 'time', 'week']

export default {
	name: 'NcDateTimePickerNative',
	inheritAttrs: false,

	props: {
		/**
		 * value attribute of the input field
		 * required
		 */
		value: {
			type: [Date, String],
			required: true,
		},
		/**
		 * id attribute of the input field
		 * required
		 */
		id: {
			type: String,
			required: true,
		},
		/**
		 * type attribute of the input field
		 * default type: String
		 * The type of the input element, it can be `date`, `datetime-local`, `month`, `time`, `week`
		 */
		type: {
			type: String,
			default: 'date',
			validate: (name) => inputDateTypes.includes(name),
		},
		/**
		 * text inside the label element
		 * default type: String
		 */
		label: {
			type: String,
			default: 'Please choose a date',
		},
		/**
		 * min attribute of the input field
		 * default type: null
		 */
		min: {
			type: [Date, Boolean],
			default: null,
		},
		/**
		 * max attribute of the input field
		 * default type: null
		 */
		max: {
			type: [Date, Boolean],
			default: null,
		},
		/**
		 * Flag to hide the label
		 * default type: String
		 * The hidden input label for accessibility purposes.
		 */
		hideLabel: {
			type: Boolean,
			default: false,
		},
	},

	emits: [
		'input',
	],

	computed: {
		formattedValue() {
			return this.formatValue(this.value)
		},
		formattedMin() {
			if (this.min) {
				return this.formatValue(this.min)
			}
			return false
		},
		formattedMax() {
			if (this.max) {
				return this.formatValue(this.max)
			}
			return false
		},
		listeners() {
			return {
				...this.$listeners,
				/**
				 * Handle the input event
				 *
				 * @param {InputEvent} $event input event payload
				 * @return {Date|string} new chosen Date() or an empty string
				 */
				input: ($event) => {
					if (isNaN($event.target.valueAsNumber)) {
						/**
						 * Emitted when the input value changes
						 *
						 * @return {string} empty string
						 */
						return this.$emit('input', '')
					}
					if (this.type === 'time') {
						const time = $event.target.value
						if (this.value === '') {
							// this case is because of Chrome bug
							const { yyyy, MM, dd } = this.getReadableDate(new Date())
							/**
							 * Emitted when the input value changes
							 *
							 * @return {Date} new chosen Date()
							 */
							return this.$emit('input', new Date(`${yyyy}-${MM}-${dd}T${time}`))
						}
						const { yyyy, MM, dd } = this.getReadableDate(this.value)
						/**
						 * Emitted when the input value changes
						 *
						 * @return {Date} new chosen Date()
						 */
						return this.$emit('input', new Date(`${yyyy}-${MM}-${dd}T${time}`))
					} else if (this.type === 'month') {
						const MM = (new Date($event.target.value).getMonth() + 1).toString().padStart(2, '0')
						if (this.value === '') {
							const { yyyy, dd, hh, mm } = this.getReadableDate(new Date())
							/**
							 * Emitted when the input value changes
							 *
							 * @return {Date} new chosen Date()
							 */
							return this.$emit('input', new Date(`${yyyy}-${MM}-${dd}T${hh}:${mm}`))
						}
						const { yyyy, dd, hh, mm } = this.getReadableDate(this.value)
						/**
						 * Emitted when the input value changes
						 *
						 * @return {Date} new chosen Date()
						 */
						return this.$emit('input', new Date(`${yyyy}-${MM}-${dd}T${hh}:${mm}`))
					}
					const timezoneOffsetSeconds = new Date($event.target.valueAsNumber).getTimezoneOffset() * 1000 * 60
					const inputDateWithTimezone = $event.target.valueAsNumber + timezoneOffsetSeconds
					/**
					 * Emitted when the input value changes
					 *
					 * @return {Date} new chosen Date()
					 */
					return this.$emit('input', new Date(inputDateWithTimezone))
				},
			}
		},
	},

	methods: {
		/**
		 * Returns Object with string values of a Date
		 *
		 * @param {Date} value The selected value
		 * @return {object}
		 */
		getReadableDate(value) {
			if (value instanceof Date) {
				const yyyy = value.getFullYear().toString().padStart(4, '0')
				const MM = (value.getMonth() + 1).toString().padStart(2, '0')
				const dd = value.getDate().toString().padStart(2, '0')
				const hh = value.getHours().toString().padStart(2, '0')
				const mm = value.getMinutes().toString().padStart(2, '0')

				return { yyyy, MM, dd, hh, mm }
			}
		},
		/**
		 * Returns preformatted value for the input field
		 *
		 * @param {Date} value The selected value
		 * @return {string}
		 */
		formatValue(value) {
			if (value instanceof Date) {
				const { yyyy, MM, dd, hh, mm } = this.getReadableDate(value)
				if (this.type === 'datetime-local') {
					return `${yyyy}-${MM}-${dd}T${hh}:${mm}`
				} else if (this.type === 'date') {
					return `${yyyy}-${MM}-${dd}`
				} else if (this.type === 'month') {
					return `${yyyy}-${MM}`
				} else if (this.type === 'time') {
					return `${hh}:${mm}`
				} else if (this.type === 'week') {
					const startDate = new Date(yyyy, 0, 1)
					const daysSinceBeginningOfYear = Math.floor((value - startDate)
						/ (24 * 60 * 60 * 1000))
					const weekNumber = Math.ceil(daysSinceBeginningOfYear / 7)
					return `${yyyy}-W${weekNumber}`
				}
			} else {
				return ''
			}
		},
	},
}
</script>

<style lang="scss" scoped>
	.native-datetime-picker {
		display: flex;
		flex-direction: column;
	}

	.native-datetime-picker .native-datetime-picker--input {
		width: 100%;
		flex: 0 0 auto;
	}
</style>
