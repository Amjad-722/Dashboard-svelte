<script>
    import { Bell, Search, Sun, Moon } from 'lucide-svelte';
    import { onMount } from 'svelte';
  import Tabs from './tabs.svelte';
    
    let isDarkMode = false;
    
    onMount(() => {
      // Check if user prefers dark mode
      if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
        enableDarkMode();
      }
      
      // Check for saved theme preference
      const savedTheme = localStorage.getItem('theme');
      if (savedTheme === 'dark') {
        enableDarkMode();
      }
    });
    
    function toggleDarkMode() {
      if (isDarkMode) {
        disableDarkMode();
      } else {
        enableDarkMode();
      }
    }
    
    function enableDarkMode() {
      document.documentElement.classList.add('dark');
      localStorage.setItem('theme', 'dark');
      isDarkMode = true;
    }
    
    function disableDarkMode() {
      document.documentElement.classList.remove('dark');
      localStorage.setItem('theme', 'light');
      isDarkMode = false;
    }
  </script>
  
  <header class="bg-white dark:bg-gray-800 border-b dark:border-gray-700 shadow-sm">
    <div class="flex items-center justify-between p-4">
      <div class="flex items-center md:w-72">
        <div class="relative w-full max-w-md md:max-w-xs">
          <div class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
            <Search size={18} class="text-gray-400" />
          </div>
          <input 
            type="search" 
            class="w-full pl-10 pr-4 py-2 rounded-md border border-gray-300 dark:border-gray-600 bg-gray-50 dark:bg-gray-700 text-gray-900 dark:text-white focus:ring-2 focus:ring-indigo-500 dark:focus:ring-indigo-600 focus:border-transparent"
            placeholder="Search..." 
          />
        </div>
      </div>
      
      <div class="flex items-center space-x-4">
        <button 
          class="p-2 rounded-full hover:bg-gray-100 dark:hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 dark:focus:ring-indigo-600"
          on:click={toggleDarkMode}
          aria-label={isDarkMode ? 'Switch to light mode' : 'Switch to dark mode'}
        >
          {#if isDarkMode}
            <Sun size={20} class="text-gray-600 dark:text-gray-300" />
          {:else}
            <Moon size={20} class="text-gray-600 dark:text-gray-300" />
          {/if}
        </button>
        
        <button 
          class="p-2 rounded-full hover:bg-gray-100 dark:hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-indigo-500 dark:focus:ring-indigo-600 relative"
          aria-label="Notifications"
        >
          <Bell size={20} class="text-gray-600 dark:text-gray-300" />
          <span class="absolute top-0 right-0 h-4 w-4 rounded-full bg-red-500 text-xs text-white flex items-center justify-center">
            3
          </span>
        </button>
        
        <div class="hidden md:flex items-center">
          
          <img 
            src="https://avatars.githubusercontent.com/u/159441246?v=4" 
            alt="User avatar" 
            class="w-8 h-8 rounded-full border-2 border-gray-200 dark:border-gray-700"
          />
        </div>
      </div>
    </div>
    
  </header>
  