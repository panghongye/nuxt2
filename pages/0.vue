<template>
	<el-date-picker
		v-model="config.model._time"
		size="small"
		type="datetimerange"
		range-separator="至"
		start-placeholder="开始时间"
		end-placeholder="结束时间"
		value-format="yyyy-MM-dd HH:mm:ss"
		:default-time="['00:00:00', '23:59:59']"
	/>
</template>

<script>
export default {
	data() {
		globalThis.t = this;
		return { config: { model: { _time: [] } } };
	},
	watch: {
		'config.model._time': {
			deep: true,
			immediate: true,
			handler(n, o) {
				console.log({ n, o });
			},
		},
	},
	methods: {
		onChange(start, end) {
			console.log('sad', start, end);
			// this.config.model._time=[start, end];
		},
	},
};
</script>

