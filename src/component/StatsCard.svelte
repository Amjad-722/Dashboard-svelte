
<script>
    import { ArrowUp, ArrowDown } from 'lucide-svelte';
    
    export let title = '';
    export let value = '';
    export let change = '';
    export let trend = 'up'; // 'up' or 'down'
  </script>
  
  <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-4">
    <div class="flex justify-between items-start">
      <div>
        <p class="text-sm font-medium text-gray-500 dark:text-gray-400">{title}</p>
        <p class="text-2xl font-semibold text-gray-900 dark:text-white mt-1">{value}</p>
      </div>
      <div class={`flex items-center px-2 py-1 rounded-full text-xs font-medium ${trend === 'up' ? 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-200' : 'bg-red-100 text-red-800 dark:bg-red-900 dark:text-red-200'}`}>
        {#if trend === 'up'}
          <ArrowUp size={14} class="mr-1" />
        {:else}
          <ArrowDown size={14} class="mr-1" />
        {/if}
        {change}
      </div>
    </div>
  </div>