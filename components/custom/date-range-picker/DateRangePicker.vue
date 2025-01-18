<script setup lang="ts">
import type { DateRange } from "radix-vue";
import { Button } from "@/components/ui/button";

import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from "@/components/ui/popover";
import { RangeCalendar } from "@/components/ui/range-calendar";
import { cn } from "@/lib/utils";
import {
  DateFormatter,
  getLocalTimeZone,
  today
} from "@internationalized/date";
import { CalendarIcon } from "@radix-icons/vue";
import { type Ref, ref } from "vue";

const df = new DateFormatter("en-US", {
  dateStyle: "medium",
});

const value = ref({
  start: today(getLocalTimeZone()),
  end: today(getLocalTimeZone()).add({ days: 3 }),
}) as Ref<DateRange>;

const props = defineProps<{
  modelValue?: DateRange | null;
}>();

const emits = defineEmits(["update:modelValue"]);

if (props.modelValue) {
  value.value = props.modelValue;
}

watch(value, (newVal) => {
  emits("update:modelValue", newVal);
});
</script>

<template>
  <Popover>
    <PopoverTrigger as-child>
      <Button
        variant="outline"
        :class="
          cn(
            'w-full justify-start text-left font-normal flex',
            !value && 'text-muted-foreground'
          )
        "
      >
        <CalendarIcon class="mr-2 h-4 w-4" />
        <template v-if="value.start">
          <template v-if="value.end">
            {{ df.format(value.start.toDate(getLocalTimeZone())) }} -
            {{ df.format(value.end.toDate(getLocalTimeZone())) }}
          </template>

          <template v-else>
            {{ df.format(value.start.toDate(getLocalTimeZone())) }}
          </template>
        </template>
        <template v-else> Pick a date </template>
      </Button>
    </PopoverTrigger>
    <PopoverContent class="w-auto p-0">
      <RangeCalendar
        v-model="value"
        initial-focus
        :number-of-months="2"
      />
    </PopoverContent>
  </Popover>
</template>
