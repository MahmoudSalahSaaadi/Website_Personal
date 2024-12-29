<script setup>
import { storeToRefs } from "pinia";
import SocialMediaIcons from "~/components/_Shared/SocialMediaIcons.vue";
import CountrySwitcher from "~/components/_Shared/CountrySwitcher.vue";
const { onLangSwitch, currentCountryLocales } = useRouteCountryHandler();

const switchLocalePath = useSwitchLocalePath();
const store = useSettingsStore();
const fetch = store.fetchSettings;
fetch();

const { loading, settings } = storeToRefs(store);

const activeZoomButton = ref("A");
const zoomReset = () => {
	document.querySelector("*").style.cssText = "zoom: normal";
};
const zoomPlus = () => {
	document.querySelector("*").style.cssText = "zoom: 110%";
	document.querySelector("*").style.cssText += "-moz-transform: scale(1.1)";
	document.querySelector("*").style.cssText += "-moz-transform-origin: left 0";
};
const zoomMinus = () => {
	document.querySelector("*").style.cssText = "zoom: 90%";
	document.querySelector("*").style.cssText += "-moz-transform: scale(0.8)";
	document.querySelector("*").style.cssText +=
		"-moz-transform-origin: center 0";
};
const zoomToggle = (size) => {
	activeZoomButton.value = size;
	if (size === "A+") {
		zoomPlus();
	} else if (size === "A") {
		zoomReset();
	} else if (size === "A-") {
		zoomMinus();
	}
};
</script>

<template>
	<section class="bg-awp-primary-400 text-white w-full py-2">
		<div class="container px-4 mx-auto flex justify-between">
			<div class="flex gap-4 items-center">
				<div class="">
					<p v-if="loading">///</p>
					<div
						v-else-if="settings.data.general[`site.hotline`]"
						class="flex gap-2 items-center headline-5"
					>
						<svg xmlns="http://www.w3.org/2000/svg" class="w-5 h-5 fill-white">
							<path
								fill="currentColor"
								d="m18.502 14.377-2.657-2.652c-.95-.947-2.563-.568-2.942.663-.285.852-1.234 1.326-2.088 1.136-1.898-.473-4.46-2.935-4.934-4.924-.285-.852.284-1.8 1.138-2.083 1.234-.379 1.614-1.989.665-2.936L5.027.93c-.76-.663-1.898-.663-2.563 0L.661 2.729c-1.803 1.894.19 6.913 4.65 11.363 4.46 4.451 9.49 6.535 11.388 4.64l1.803-1.799c.664-.757.664-1.894 0-2.556Z"
							/>
						</svg>
						<a :href="`tel:${settings.data.general[`site.hotline`]}`">{{
							settings.data.general[`site.hotline`]
						}}</a>
					</div>
				</div>

				<div
					v-if="!loading && settings.data.general[`site.hotline`]"
					class="w-px bg-white h-5"
				/>

				<SocialMediaIcons />
			</div>

			<div class="flex items-center gap-4">
				<div>
					<button
						v-for="size in ['A+', 'A', 'A-']"
						:key="size"
						class="w-8 h-8 font-bold rounded-sm"
						:class="[activeZoomButton === size ? 'bg-white/50' : '']"
						type="button"
						dir="ltr"
						@click="zoomToggle(size)"
					>
						{{ size }}
					</button>
				</div>

				<div class="w-px bg-white h-5"></div>

				<a
					v-for="lang in currentCountryLocales"
					:key="lang.code"
					class="flex items-center gap-2"
					:href="switchLocalePath(lang.code)"
					@click.prevent="onLangSwitch(switchLocalePath(lang.code))"
				>
					<IconsGlobe />
					{{ lang.name }}
				</a>

				<div class="w-px bg-white h-5"></div>
				<CountrySwitcher />
			</div>
		</div>
	</section>
</template>
