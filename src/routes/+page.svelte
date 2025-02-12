<script lang="ts">
	import { Line } from 'svelte-chartjs';
	import { Pie } from 'svelte-chartjs';
	import { data } from '$lib/data.js';

	import {
		Chart as ChartJS,
		Title,
		Tooltip,
		Legend,
		LineElement,
		LinearScale,
		PointElement,
		ArcElement,
		CategoryScale,
		elements
	} from 'chart.js';

	ChartJS.register(
		Title,
		Tooltip,
		Legend,
		LineElement,
		LinearScale,
		ArcElement,
		PointElement,
		CategoryScale
	);
	type ChartData = {
		labels: (string | number)[];
		datasets: {
			label?: string;
			fill?: boolean;
			borderColor?: string;
			data?: number[];
			backgroundColor?: string[];
			hoverBackgroundColor?: string[];
		}[];
	};
	function getAverage(array: number[]) {
		let sum = 0;
		for (let i = 0; i < array.length; i++) {
			sum += array[i];
		}
		return sum / array.length;
	}
	let output = 0;
	let output2 = 0;
	data.forEach((element) => (output += element.f));
	data.forEach((element) => (output2 += element.m));
	output = output / data.length;
	const output3 = (output + output2) / 2;
	const chart1: ChartData = {
		labels: data.map((element) => element.year),
		datasets: [
			{
				label: 'Male',
				fill: false,
				borderColor: 'rgb(54, 162, 235)',
				data: data.map((element) => element.m)
			},
			{
				label: 'Female',
				fill: false,
				borderColor: 'rgb(255, 99, 132)',
				data: data.map((element) => element.f)
			},
			{
				label: 'Average',
				fill: false,
				borderColor: 'rgb(100, 99, 132)',
				data: data.map((element) => (element.m + element.f) / 2)
			},
			{
				label: 'Line',
				fill: false,
				borderColor: 'rgb(70, 255, 255)',
				data: Array(data.length).fill(output3)
			}
		]
	};

	let chart2: ChartData = {
		labels: ['Male', 'Female'],
		datasets: [
			{
				data: [12, 12],

				backgroundColor: ['rgb(12, 123, 123', 'rgb(123,12,123)']
			}
		]
	};
</script>

<div class="container mx-auto">
	<div class="grid grid-cols-2 gap-10 mb-10">
		<div class="border-4 border-violet-200 p-3">
			<h2 class="text-center text-2xl text-violet-700 font-bold">Graf podatkov</h2>
			<Line data={chart1} options={{ responsive: true }} />
		</div>
		<div class="border-4 border-violet-200 p-3">
			<h2 class="text-center text-2xl text-violet-700 font-bold">Graf podatkov</h2>
			<Pie data={chart2} options={{ responsive: true }} />
		</div>
	</div>
</div>
