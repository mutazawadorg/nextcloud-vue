<!--
  - @copyright 2022 Christopher Ng <chrng8@gmail.com>
  -
  - @author Christopher Ng <chrng8@gmail.com>
  -
  - @license AGPL-3.0-or-later
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

<template>
	<li class="resource">
		<NcButton class="resource__button"
			:aria-label="labelTranslated"
			type="tertiary"
			:href="url">
			<template #icon>
				<div class="resource__icon">
					<img :src="icon">
				</div>
			</template>
			{{ title }}
		</NcButton>
	</li>
</template>

<script>
import NcButton from '../NcButton/index.js'
import Tooltip from '../../directives/Tooltip/index.js'
import { t } from '../../l10n.js'

export default {
	name: 'NcResource',

	components: {
		NcButton,
	},

	directives: {
		Tooltip,
	},

	props: {
		icon: {
			type: String,
			required: true,
		},
		title: {
			type: String,
			required: true,
		},
		subtitle: {
			type: String,
			default: null,
		},
		tooltip: {
			type: String,
			default: null,
		},
		url: {
			type: String,
			required: true,
		},
	},

	data() {
		return {
			labelTranslated: t('Open link to "{resourceTitle}"', { resourceTitle: this.title }),
		}
	},
}
</script>

<style lang="scss" scoped>
.resource {
	display: flex;
	align-items: center;
	height: 44px;

	// Override default NcButton styles
	&__button {
		width: 100% !important;
		justify-content: flex-start !important;
		padding: 0 !important;

		&:deep(.button-vue__text) {
			font-weight: normal !important;
			margin-left: 2px !important;
		}
	}

	&__icon {
		width: 32px;
		height: 32px;
		background-color: var(--color-text-maxcontrast);
		border-radius: 50%;
		display: flex;
		align-items: center;
		justify-content: center;

		img {
			width: 16px;
			height: 16px;
			filter: var(--background-invert-if-dark);
		}
	}
}
</style>
