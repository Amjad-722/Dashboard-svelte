<script>
    export let activities = [];
  </script>
  
  <div class="divide-y divide-gray-200 dark:divide-gray-700">
    {#each activities as activity}
      <div class="p-4">
        <div class="flex items-start">
          <div class="flex-shrink-0 mr-3">
            <div class="w-8 h-8 rounded-full bg-indigo-100 dark:bg-indigo-900 flex items-center justify-center text-indigo-500 dark:text-indigo-300">
              {activity.user.charAt(0)}
            </div>
          </div>
          <div>
            <p class="text-sm text-gray-800 dark:text-gray-200">
              <span class="font-medium">{activity.user}</span> {activity.action}
            </p>
            <p class="text-xs text-gray-500 dark:text-gray-400 mt-1">
              {activity.time}
            </p>
          </div>
        </div>
      </div>
    {/each}
  </div>