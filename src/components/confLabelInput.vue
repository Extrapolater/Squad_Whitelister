<script setup lang="ts"></script>

<script lang="ts">
	import { anyTypeAnnotation } from '@babel/types';
	import { stringifyStyle } from '@vue/shared';

	export default {
		data() {
			return {
				content: this.modelValue,
				config_tr: {
					web_server: 'Web Server',
					app_personalization: 'Personalization',
				} as any,
			};
		},
		name: 'confLabelInput',
		props: {
			modelValue: null as any,
			confKey: {
				required: true,
				type: String,
			},
		},
		methods: {
			log: console.log,
			handleInput: function (e: any) {
				let ret = e.target.value;
				if (e.target.type == 'checkbox') ret = e.target.checked;
				this.$emit('update:modelValue', ret);
			},
			getInputType: function (o: any) {
				const tmpType = {
					string: 'text',
					number: 'number',
					boolean: 'checkbox',
				} as any;
				return o.toString().startsWith('#') ? 'color' : tmpType[typeof o];
			},
			getTranslation: function (t: string) {
				const trC = this.config_tr[t];
				return trC ? trC : this.toUpperFirstChar(t.replace(/\_/g, ' '));
			},
			toUpperFirstChar: function (string: string) {
				return string.charAt(0).toUpperCase() + string.slice(1);
			},
			convHex6digs: function (hex: any) {
				if (hex.toString().startsWith('#')) {
					return '#' + hex[1] + hex[1] + hex[2] + hex[2] + hex[3] + hex[3];
				} else return hex;
			},
		},
		created() {
			// console.log('conf_data:', this.confKey, this.modelValue);
		},
	};
</script>

<template>
	<fieldset v-if="typeof content == 'object'">
		<legend>{{ toUpperFirstChar(confKey.replace(/\_/g, ' ')) }}</legend>
		<!-- <div v-if="typeof content == 'object'" style="background: #ffffff08"> -->
		<!-- <h3>{{ toUpperFirstChar(confKey.replace(/\_/g, ' ')) }}</h3> -->
		<confLabelInput v-for="k of Object.keys(content)" :key="k" :confKey="k" :modelValue="content[k]" @update:modelValue="(nv) => (content[k] = nv)" />
		<!-- </div> -->
	</fieldset>
	<label v-else>{{ getTranslation(confKey) }}<input :type="getInputType(content)" :value="modelValue" :checked="modelValue" @input="handleInput" /></label>
</template>

<style scoped>
	label {
		flex-wrap: nowrap;
		white-space: nowrap;
		margin-bottom: 10px;
	}
	label > input,
	label > select {
		margin: 0;
		margin-left: 10px;
	}
	div.ct {
		margin: 0 auto;
	}
	h2,
	h3 {
		margin-bottom: 20px;
		border-bottom: 2px solid;
	}
</style>
