<script lang="ts">
	import profile from '$lib/assets/karsten-winegeart-uNVjLyA3xu8-unsplash.jpg';
	// @ts-ignore
	import { AlertCircle, BellIcon, CheckCheckIcon, ListFilterIcon } from 'lucide-svelte';

	import {
		Avatar,
		WidgetPlaceholder,
		Skeleton,
		Table,
		Card,
		TableBody,
		TableBodyCell,
		TableBodyRow,
		TableHead,
		TableHeadCell
	} from 'flowbite-svelte';
	import Chart from '$lib/components/Chart.svelte';
	import Radar from '$lib/components/Radar.svelte';
	import Line from '$lib/components/Line.svelte';
	import { navigating } from '$app/stores';

	import { page } from '$app/stores';
	import UserChart from '$lib/components/UserChart.svelte';
	import { slide } from 'svelte/transition';

	type Payments = {
		name: string;
		meter: string;
		january: number;
		febuary: number;
		march: number;
		april: number;
		may: number;
		june: number;
		july: number;
		august: number;
		september: number;
		october: number;
		november: number;
		december: number;
		address: string;
		status: string;
		graph: number[];
	}[];

	let payments: Payments = $page.data.info; 

	let suspects = payments.filter((item) => item.status == 'suspect');

	export let data;

	let { brief } = data;

	const calcSum = (i: number[]) => {
		let sum = 0;

		for (let index = 0; index < i.length; index++) {
			sum += i[index];
		}

		return sum;
	};

	const calcAverage = (i: number[]) => {
		let sum = calcSum(i);

		return sum / i.length;
	};

	let searchTerm: string = 'suspect';

	$: filteredItems = brief?.filter((item) => item.status.indexOf(searchTerm.toLowerCase()) !== -1);
</script>

{#if $navigating}
	<div class=" space-y-8">
		<Skeleton />
		<WidgetPlaceholder />
	</div>
{:else}

	<div class=" mx-auto mt-10 w-[95%]">
		<div class="  flex w-full flex-col space-y-10">
			<div class=" ">
				<h1 class=" text-4xl  font-bold">MARKET STRATEGY ANALYSIS PLATFORM</h1>
			</div>
			<div class=" grid w-full grid-cols-3 gap-6">
				<div class=" min-h-[50vh] pt-[3em] w-full">
					<Radar suspect={suspects.length} non_suspect={payments.length - suspects.length} />
				</div>
				<div class=" col-span-2 min-h-[50vh] w-full ">
					<h1 class=" pl-6 text-lg font-medium">Revenue</h1>
					<Line />
				</div>

<!--				<div class="  col-span-2 min-h-[50vh]  w-full ">-->
<!--					<Table color="custom" customeColor="#262d47" divClass=" bg-red-400"  class="  bg-red-400 py-8">-->
<!--						<TableHead theadClass=" bg-[#262d47]"  class="">-->
<!--							<TableHeadCell>Meter No.</TableHeadCell>-->
<!--							<TableHeadCell>Name</TableHeadCell>-->
<!--							<TableHeadCell>Average Amount</TableHeadCell>-->
<!--							<TableHeadCell>Total Amount</TableHeadCell>-->
<!--							<TableHeadCell>No. Payments</TableHeadCell>-->
<!--							<TableHeadCell>Status</TableHeadCell>-->
<!--						</TableHead>-->
<!--						<TableBody  tableBodyClass="divide-y bg-[#262d47]">-->
<!--							{#each filteredItems as item, i}-->
<!--								<TableBodyRow>-->
<!--									<TableBodyCell>{item.meter}</TableBodyCell>-->
<!--									<TableBodyCell>{item.name}</TableBodyCell>-->
<!--									<TableBodyCell>{Math.floor(calcAverage(item.graph))}</TableBodyCell>-->
<!--									<TableBodyCell>{calcSum(item.graph)}</TableBodyCell>-->
<!--									<TableBodyCell>{item.graph.length}</TableBodyCell>-->
<!--									<TableBodyCell>-->
<!--										{#if item.status == 'suspect'}-->
<!--											<button-->
<!--												class=" flex items-center space-x-2 rounded-full border-2 border-red-500 px-4 py-2 text-red-500"-->
<!--											>-->
<!--												<AlertCircle />-->
<!--												<h4>Suspect</h4>-->
<!--											</button>-->
<!--										{:else}-->
<!--											<button-->
<!--												class=" flex items-center space-x-2 rounded-full border-2 border-green-500 px-4 py-2 text-green-500"-->
<!--											>-->
<!--												<CheckCheckIcon />-->
<!--												<h4>Non Suspect</h4>-->
<!--											</button>-->
<!--										{/if}-->
<!--									</TableBodyCell>-->
<!--								</TableBodyRow>-->
<!--							{/each}-->
<!--						</TableBody>-->
<!--					</Table>-->
<!--				</div>-->

				<div class=" col-span-2 min-h-[50vh] w-full ">
					<Chart />
				</div>
			</div>
		</div>
	</div>
{/if}
