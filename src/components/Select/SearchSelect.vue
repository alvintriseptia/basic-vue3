<template>
	<el-select
		v-model="value"
		multiple
		filterable
		remote
		reserve-keyword
		placeholder="Please enter a keyword"
		remote-show-suffix
		:remote-method="remoteMethod"
		:loading="loading"
		class="w-[300px]"
	>
		<el-option
			v-for="item in options"
			:key="item.value"
			:label="item.label"
			:value="item.value"
		/>
	</el-select>
</template>

<script setup>
import { defineProps, onMounted, ref } from "vue";

const loading = ref(false);
const list = ref([]);
const options = ref([]);
const value = ref([]);

const props = defineProps({
	options: {
		type: Array,
		default: () => [],
	},
});

onMounted(() => {
	list.value = props.options.map((item) => {
		return { value: `value:${item.value}`, label: `label:${item.label}` };
	});

	options.value = list.value.slice(0, 3);
});

const remoteMethod = (query) => {
	if (query) {
		loading.value = true;
		setTimeout(() => {
			loading.value = false;
			options.value = list.value.filter((item) => {
				return item.label.toLowerCase().includes(query.toLowerCase());
			});
		}, 200);
	} else {
		options.value = list.value.slice(0, 3);
	}
};
</script>
