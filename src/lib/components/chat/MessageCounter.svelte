<script lang="ts">
  import { onDestroy, onMount } from "svelte";
  import { createEventDispatcher } from "svelte";
  
  export let history: any;
  export let maxTurns: number = 30; // Standard-Wert, kann überschrieben werden

  const dispatch = createEventDispatcher();
  
  let userMessages = 0;
  let remainingMessages = maxTurns;
  let progressPercentage = 0;
  
  function countUserMessages(messages) {
    if (!messages || !Object.values(messages)) return 0;
    return Object.values(messages).filter(msg => msg.role === 'user').length;
  }

  function updateCounter() {
    if (history && history.messages) {
      userMessages = countUserMessages(history.messages);
      remainingMessages = Math.max(0, maxTurns - userMessages);
      progressPercentage = Math.min(100, Math.round((userMessages / maxTurns) * 100));
    }
  }

  $: if (history && history.messages) {
    updateCounter();
  }

  onMount(() => {
    updateCounter();
  });
</script>

<div class="message-counter">
  <div class="flex flex-col items-center">
    <!-- Counter text first -->
    <div class="flex items-center justify-center text-sm bg-opacity-80 rounded-full py-1 px-4 shadow-sm w-full mb-1 {remainingMessages <= 5 ? 'bg-amber-100 text-amber-800 dark:bg-amber-900 dark:text-amber-200' : 'bg-blue-100 text-blue-800 dark:bg-blue-900 dark:text-blue-200'}">
      <span class="font-medium">{remainingMessages}</span>
      <span class="ml-1">Nachricht{remainingMessages !== 1 ? 'en' : ''} übrig</span>
    </div>
    
    <!-- Progress bar with reversed colors -->
    <div class="w-full bg-gray-200 rounded-full h-2 dark:bg-gray-700 overflow-hidden">
      <div 
        class="h-2 rounded-full {progressPercentage < 25 ? 'bg-red-500' : progressPercentage < 50 ? 'bg-amber-500' : progressPercentage < 75 ? 'bg-yellow-500' : 'bg-green-500'}" 
        style="width: {progressPercentage}%">
      </div>
    </div>
  </div>
</div>

<style>
  .message-counter {
    position: fixed;
    bottom: 110px;
    right: 20px;
    z-index: 50;
    width: 180px;
    filter: drop-shadow(0 1px 2px rgba(0, 0, 0, 0.1));
  }
</style>