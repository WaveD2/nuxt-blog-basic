<template>
  <div class="hidden min-w-16 xs:block xs:col-span-1 xl:col-span-2">
    <div class="sticky top-0">
      <div class="h-screen flex flex-col">
        <div
          :class="defaultTransition"
          class="p-2 ms-3 max-w-min my-2 rounded-full hover:bg-blue-50 dark:hover:bg-slate-50/20">
          <RouterLink class="h-8 w-8" to="/">
            <IconLogo class="h-8 w-8" />
          </RouterLink>
        </div>
        <div class="flex flex-col gap-2">
          <SidebarTab link="/" active>
            <template #icon>
              <HomeIcon class="h-8 w-8" />
            </template>
            <template #nama> Home </template>
          </SidebarTab>

          <SidebarTab>
            <template #icon>
              <InboxIcon class="h-8 w-8" />
            </template>
            <template #nama> Messages </template>
          </SidebarTab>

          <SidebarTab>
            <template #icon>
              <DocumentTextIcon class="h-8 w-8" />
            </template>
            <template #nama> List </template>
          </SidebarTab>
          <SidebarTab>
            <template #icon>
              <UserIcon class="h-8 w-8" />
            </template>
            <template #nama> Profile </template>
          </SidebarTab>
          <SidebarTab>
            <template #icon>
              <EllipsisHorizontalCircleIcon class="h-8 w-8" />
            </template>
            <template #nama> More </template>
          </SidebarTab>
          <div class="xl:hidden px-3">
            <UIButton @click="emits('onTweet')">
              <div class="w-6 h-6 font-bold"><PencilIcon /></div
            ></UIButton>
          </div>
          <div class="max-xl:hidden">
            <UIButton @click="emits('onTweet')" size="lg" liquid>
              <span class="font-bold">Tweet</span>
            </UIButton>
          </div>
        </div>
        <div
          @click="emits('onLogout')"
          :class="defaultTransition"
          class="flex flex-row items-center justify-center px-2 py-2 mx-auto mt-auto mb-5 rounded-full cursor-pointer w-14 xl:w-full hover:bg-gray-100 dark:hover:bg-dim-800">
          <div class="flex flex-row items-center">
            <img
              :src="props.user?.profileImage"
              class="h-10 w-10 rounded-full"
              alt="" />
            <div class="flex-col hidden ml-2 xl:flex">
              <h1 class="text-sm font-bold text-gray-800 dark:text-white">
                {{ user?.name }}
              </h1>
              <p class="text-sm text-gray-400">@{{ user?.username }}</p>
            </div>
          </div>
          <div class="hidden ml-auto xl:block dark:text-white">
            <div class="w-6 h-6">
              <ChevronDownIcon />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
  const props = defineProps({
    user: {
      type: Object,
      required: true,
    },
  });
  import {
    HashtagIcon,
    BellIcon,
    InboxIcon,
    BookmarkIcon,
    DocumentTextIcon,
    UserIcon,
    EllipsisHorizontalCircleIcon,
    PencilIcon,
    ChevronDownIcon,
  } from "@heroicons/vue/24/outline";
  import {HomeIcon} from "@heroicons/vue/24/solid";

  const emits = defineEmits(["onTweet", "onLogout"]);
  const {defaultTransition} = useTailwindConfig();
</script>
