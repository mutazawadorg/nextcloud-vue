<!--
 - @copyright Copyright (c) 2019 Marco Ambrosini <marcoambrosini@icloud.com>
 -
 - @author Marco Ambrosini <marcoambrosini@icloud.com>
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

### Normal Counter

```
<NcCounterBubble>314+</NcCounterBubble>
```

### Outlined Counter (e.g team mentions)

```
<NcCounterBubble type="outlined">314+</NcCounterBubble>
```

### Highlighted Counter (e.g direct mentions)

```
<NcCounterBubble type="highlighted">314+</NcCounterBubble>
```

</docs>

<template>
	<div :class="counterClassObject"
		class="counter-bubble__counter">
		<slot />
	</div>
</template>

<script>

export default {
	name: 'NcCounterBubble',

	props: {
		type: {
			type: String,
			default: '',
			validator(value) {
				return ['highlighted', 'outlined', ''].indexOf(value) !== -1
			},
		},
	},

	computed: {
		counterClassObject() {
			return {
				'counter-bubble__counter--highlighted': this.type === 'highlighted',
				'counter-bubble__counter--outlined': this.type === 'outlined',
			}
		},
	},
}

</script>

<style lang="scss" scoped>
.counter-bubble__counter {
	font-size: calc(var(--default-font-size) * .8);
	overflow: hidden;
	width: fit-content;
	max-width: $clickable-area;
	text-align: center;
	text-overflow: ellipsis;
	line-height: 1em;
	padding: 4px 6px;
	border-radius: var(--border-radius-pill);
	// since -lighter is not present in the new version it will only apply to the old one and -light for the newer
	background-color: var(--color-primary-element-lighter, var(--color-primary-element-light));
	font-weight: bold;
	color: var(--color-primary-element);

	&--highlighted {
		color: var(--color-primary-text);
		background-color: var(--color-primary);
	}

	&--outlined {
		color: var(--color-primary);
		background: transparent;
		box-shadow: inset 0 0 0 2px;
	}
}

</style>
