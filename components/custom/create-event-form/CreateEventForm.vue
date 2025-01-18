<template>
  <div>
    <Card class="w-full lg:max-w-2xl mx-auto">
      <CardHeader>
        <CardTitle class="flex flex-row items-center gap-2">
          <CalendarPlus class="size-[19px]" />
          Create Event
        </CardTitle>
        <CardDescription>
          Schedule an event with your friends, family, or colleagues by filling
          in the details below.
        </CardDescription>
      </CardHeader>
      <CardContent class="space-y-2">
        <div class="space-y-1">
          <Label for="title" class="font-semibold">Title</Label>
          <Input
            id="title"
            type="text"
            v-model="title"
            placeholder="Enter Title"
            class="border rounded px-2 py-1 text-sm w-full"
          />
          <p v-if="errors.title" class="text-red-600 text-sm">
            {{ errors.title }}
          </p>
        </div>
        <div class="space-y-1">
          <Label for="date-range" class="font-semibold">Date Range</Label>
          <DateRangePicker id="date-range" v-model="dateRange" />
          <p v-if="errors.dateRange" class="text-red-600 text-sm">
            {{ errors.dateRange }}
          </p>
        </div>

        <div class="flex flex-row justify-between">
          <div class="space-y-1">
            <Label for="start-time" class="font-semibold"> Start Time </Label>
            <div class="flex space-x-2">
              <Select id="start-time" v-model="startTime">
                <SelectTrigger class="w-30">
                  <SelectValue placeholder="Select Time" />
                </SelectTrigger>
                <SelectContent>
                  <SelectGroup>
                    <SelectItem
                      v-for="hour in hours"
                      :key="hour + '-start-00'"
                      :value="formatHour(hour) + ':00'"
                    >
                      {{ formatHour(hour) }}:00
                    </SelectItem>
                    <SelectItem
                      v-for="hour in hours"
                      :key="hour + '-start-30'"
                      :value="formatHour(hour) + ':30'"
                    >
                      {{ formatHour(hour) }}:30
                    </SelectItem>
                  </SelectGroup>
                </SelectContent>
              </Select>

              <Select v-model="startTimeMeridiem">
                <SelectTrigger class="w-24">
                  <SelectValue placeholder="AM/PM" />
                </SelectTrigger>
                <SelectContent>
                  <SelectGroup>
                    <SelectItem value="AM">AM</SelectItem>
                    <SelectItem value="PM">PM</SelectItem>
                  </SelectGroup>
                </SelectContent>
              </Select>
            </div>
            <p v-if="errors.startTime" class="text-red-600 text-sm mt-1">
              {{ errors.startTime }}
            </p>
          </div>

          <div class="space-y-1">
            <Label for="end-time" class="font-semibold"> End Time </Label>
            <div class="flex space-x-2">
              <Select id="end-time" v-model="endTime">
                <SelectTrigger class="w-30">
                  <SelectValue placeholder="Select Time" />
                </SelectTrigger>
                <SelectContent>
                  <SelectGroup>
                    <SelectItem
                      v-for="hour in hours"
                      :key="hour + '-end-00'"
                      :value="formatHour(hour) + ':00'"
                    >
                      {{ formatHour(hour) }}:00
                    </SelectItem>
                    <SelectItem
                      v-for="hour in hours"
                      :key="hour + '-end-30'"
                      :value="formatHour(hour) + ':30'"
                    >
                      {{ formatHour(hour) }}:30
                    </SelectItem>
                  </SelectGroup>
                </SelectContent>
              </Select>

              <Select v-model="endTimeMeridiem">
                <SelectTrigger class="w-24">
                  <SelectValue placeholder="AM/PM" />
                </SelectTrigger>
                <SelectContent>
                  <SelectGroup>
                    <SelectItem value="AM">AM</SelectItem>
                    <SelectItem value="PM">PM</SelectItem>
                  </SelectGroup>
                </SelectContent>
              </Select>
            </div>
            <p v-if="errors.endTime" class="text-red-600 text-sm mt-1">
              {{ errors.endTime }}
            </p>
          </div>
        </div>
      </CardContent>

      <CardFooter>
        <Button
          @click="handleSubmit"
          class="bg-violet-500 hover:bg-violet-600 text-white w-full"
        >
          Create Event
        </Button>
      </CardFooter>
    </Card>
  </div>
</template>

<script setup lang="ts">
import { ref, reactive } from "vue";
import {
  Card,
  CardHeader,
  CardTitle,
  CardDescription,
  CardContent,
} from "@/components/ui/card";
import {
  Select,
  SelectTrigger,
  SelectValue,
  SelectContent,
  SelectGroup,
  SelectItem,
} from "@/components/ui/select";
import type { TablesInsert } from "~/types/database.types";
import { CalendarPlus } from "lucide-vue-next";
import { Button } from "@/components/ui/button";
import type { DateRange } from "radix-vue";
import { getLocalTimeZone, today } from "@internationalized/date";

const title = ref("");
const dateRange = ref({
  start: today(getLocalTimeZone()),
  end: today(getLocalTimeZone()).add({ days: 3 }),
}) as Ref<DateRange | null>;
const startTime = ref("");
const startTimeMeridiem = ref("");
const endTime = ref("");
const endTimeMeridiem = ref("");

interface Errors {
  title?: string;
  dateRange?: string;
  startTime?: string;
  endTime?: string;
}
const errors = reactive<Errors>({});

const hours = [7, 8, 9, 10, 11, 12, 1, 2, 3, 4, 5, 6];

function formatHour(hour: number): string {
  return hour.toString().padStart(2, "0");
}

const supabase = useSupabaseClient();
const user = useSupabaseUser();
const router = useRouter();

async function generateEventCode(): Promise<string | null> {
  let code = "";
  let exists = true;
  const characters =
    "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";

  while (exists) {
    code = "";
    for (let i = 0; i < 6; i++) {
      code += characters.charAt(Math.floor(Math.random() * characters.length));
    }

    const { data, error } = await supabase
      .from("events")
      .select("code")
      .eq("code", code);

    if (error) {
      console.error("Error checking event code:", error);
      return null;
    }

    if (data.length === 0) {
      exists = false;
    }
  }

  return code;
}

async function handleSubmit() {
  const newErrors: Errors = {};

  if (!title.value) {
    newErrors.title = "Title is required.";
  }
  if (!dateRange.value?.start || !dateRange.value?.end) {
    newErrors.dateRange = "Date range is required.";
  }
  if (!startTime.value || !startTimeMeridiem.value) {
    newErrors.startTime = "Start time is required.";
  }
  if (!endTime.value || !endTimeMeridiem.value) {
    newErrors.endTime = "End time is required.";
  }

  Object.assign(errors, newErrors);

  if (Object.keys(newErrors).length === 0) {
    try {
      const eventCode = await generateEventCode();
      if (!eventCode) {
        alert("Failed to generate event code.");
        return;
      }

      const { start, end } = dateRange.value!;

      const startTimeFormatted = `${startTime.value} ${startTimeMeridiem.value}`;
      const endTimeFormatted = `${endTime.value} ${endTimeMeridiem.value}`;

      const newEvent: TablesInsert<"events"> = {
        creator_user_id: user?.value?.id,
        title: title.value,
        start_date: start!.toString(),
        end_date: end!.toString(),
        start_time: startTimeFormatted,
        end_time: endTimeFormatted,
        code: eventCode,
      };

      const { data, error } = await supabase.from("events").insert(newEvent);

      if (error) {
        console.error("Error inserting event:", error.message);

        alert("Failed to create the event. Please try again.");
      } else {
        console.log("Event created successfully:", data);

        alert("Event created successfully!");
        router.push(`/events/${eventCode}`);
      }
    } catch (err) {
      console.error("Unexpected error:", err);
      alert("An unexpected error occurred. Please try again.");
    }
  }
}
</script>

<style scoped></style>
