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
<NcAppNavigationCounter>314+</NcAppNavigationCounter>
```

### Highlighted Counter (i.e. mentions)

```
<NcAppNavigationCounter :highlighted="true">@admin</NcAppNavigationCounter>
<NcAppNavigationCounter :highlighted="true">314+</NcAppNavigationCounter>
```

</docs>

<template>
	<div :class="{ 'app-navigation-entry__counter--highlighted': highlighted }"
		class="app-navigation-entry__counter">
		<slot />
	</div>
</template>

<script>

/**
 * @deprecated use [NcCounterBubble](#/Components/NcCounterBubble) instead
 */
export default {
	name: 'NcAppNavigationCounter',

	props: {
		highlighted: {
			type: Boolean,
			default: false,
		},
	},
}

</script>

<style lang="scss" scoped>
.app-navigation-entry__counter {
	font-size: calc(var(--default-font-size) * .8);
	overflow: hidden;
	width: fit-content;
	max-width: $clickable-area;
	text-align: center;
	text-overflow: ellipsis;
	line-height: 1em;
	padding: 4px 8px;
	border-radius: var(--border-radius-pill);
	background-color: var(--color-background-darker);

	&--highlighted {
		padding: 4px 6px;
		color: var(--color-primary-text);
		background-color: var(--color-primary);
	}
}

</style>
