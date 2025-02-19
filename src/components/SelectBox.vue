<script setup lang="ts">
const props = withDefaults(defineProps<{
    list: {
        label: string,
        value: string
    }[],
    picked?: string,
	color?: string
}>(), {
	picked: (p) => p.list[0].value,
	color: 'bg-base-100'
})

const selectedElement = ref(props.list.find(v => v.value === props.picked) ?? props.list[0])

const emit = defineEmits<{
	(e: 'update', value: typeof props.list[number]): void
}>()

defineExpose({
    selectedElement
})
</script>

<template>
	<Listbox v-model="selectedElement" by="value" @update:modelValue="value => emit('update', value)">
		<div class="relative rounded-lg">
			<ListboxButton :class="[ color ]"
				class="flex w-full cursor-default items-center justify-between gap-1 rounded-md p-2 text-left text-sm">
				<span class="block truncate font-semibold">{{ selectedElement.label }}</span>
				<heroicons-chevron-up-down-20-solid class="h-6 w-6" />
			</ListboxButton>
			<Transition enterActiveClass="transition duration-200 ease-out"
				enterFromClass="transform opacity-0" enterToClass="transform opacity-100"
				leaveActiveClass="transition duration-200 ease-in" leaveFromClass="transform opacity-100"
				leaveToClass="transform opacity-0">
				<ListboxOptions :class="[ color ]"
					class="join join-vertical absolute z-10 mt-4 w-full min-w-fit overflow-auto rounded-md text-sm shadow-lg">
					<ListboxOption v-for="element in list" :key="element.value" v-slot="{ active, selected }" as="template" :value="element">
						<li :class="[
							active ? 'bg-primary !text-base-100' : '',
							selected ? 'bg-primary-focus font-semibold text-base-100' : 'text-neutral',
							'join-item relative cursor-default select-none px-3 py-2',
						]">
							<span class="block truncate">{{ element.label }}</span>
						</li>
					</ListboxOption>
				</ListboxOptions>
			</Transition>
		</div>
	</Listbox>
</template>
