<script lang="ts" setup>
import { useStore } from "~/store";
const store = useStore();
const handleKeypress = (e: KeyboardEvent) => {
  if (e.code == "KeyE") {
    store.onRestore();
  }
  if (e.code == "KeyR") {
    store.onArchivedMarkAsRead();
  }
};

onMounted(() => {
  document.addEventListener("keypress", handleKeypress);
});

onUnmounted(() => {
  document.removeEventListener("keypress", handleKeypress);
});
</script>

<template>
  <div class="inbox-page">
    <header>
      <h2>Archive</h2>
    </header>
    <section class="inbox-page__top-nav">
      <CheckBox
        @onChange="store.onSelectAllArchived()"
        :checked="store.isSelectedAllArchived"
      >
        Email Selected ({{ store.selectedArchivedEmailsCount }})
      </CheckBox>
      <ul>
        <li>
          <img
            height="19"
            width="19"
            src="~/assets/images/mail.svg"
            alt="email"
          />
          <span>Mark as read (r)</span>
        </li>
        <li>
          <img
            height="19"
            width="19"
            src="~/assets/images/restore.svg"
            alt="archive"
          />
          <span>Restore (e)</span>
        </li>
      </ul>
    </section>
    <transition name="fade" mode="out-in">
      <template v-if="store.archivedEmails.length">
        <ul class="inbox-page__list">
          <transition-group name="fade" mode="out-in">
            <li
              v-for="(item, index) in store.archivedEmails"
              :key="index"
              class="inbox-page__list__item"
              :class="item.markRead && 'mark-as-read'"
            >
              <CheckBox
                :checked="item.checked"
                @onChange="item.checked = !item.checked"
              >
                {{ item.title }}
              </CheckBox>
            </li>
          </transition-group>
        </ul>
      </template>
      <div v-else class="no-data">No Archived Emails</div>
    </transition>
  </div>
</template>

<style scoped></style>
