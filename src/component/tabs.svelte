<script>
    import { onMount } from 'svelte';
    import { ChevronDownIcon } from '@heroicons/svelte/16/solid';
    import { BuildingOfficeIcon, CreditCardIcon, UserIcon, UsersIcon } from '@heroicons/svelte/20/solid';
  
    export let tabs = [
      { name: 'My Account', href: '#', icon: UserIcon, current: false },
      { name: 'Company', href: '#', icon: BuildingOfficeIcon, current: false },
      { name: 'Team Members', href: '#', icon: UsersIcon, current: true },
      { name: 'Billing', href: '#', icon: CreditCardIcon, current: false },
    ];
  
    let selectedTab = tabs.find(tab => tab.current)?.name || tabs[0].name;
  
    function handleSelect(event) {
      selectedTab = event.target.value;
      const selected = tabs.find(tab => tab.name === selectedTab);
      if (selected) {
        window.location.href = selected.href;
      }
    }
  </script>
  
  <div class="w-full max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    <!-- Mobile Dropdown -->
    <div class="grid grid-cols-1 sm:hidden relative">
      <select
        bind:value={selectedTab}
        on:change={handleSelect}
        aria-label="Select a tab"
        class="col-start-1 row-start-1 w-full appearance-none rounded-md bg-white py-2 pl-3 pr-8 text-base text-gray-900 outline outline-1 -outline-offset-1 outline-gray-300 focus:outline focus:outline-2 focus:-outline-offset-2 focus:outline-indigo-600"
      >
        {#each tabs as tab}
          <option key={tab.name}>{tab.name}</option>
        {/each}
      </select>
      <ChevronDownIcon
        aria-hidden="true"
        class="pointer-events-none col-start-1 row-start-1 mr-2 size-5 self-center justify-self-end fill-gray-500"
      />
    </div>
  
    <!-- Desktop Tabs -->
    <div class="hidden sm:block">
      <div class="border-b border-gray-200">
        <nav aria-label="Tabs" class="-mb-px flex flex-wrap justify-center space-x-4 sm:space-x-8">
          {#each tabs as tab}
            <a
              href={tab.href}
              aria-current={tab.current ? 'page' : undefined}
              class="group inline-flex items-center border-b-2 px-3 py-4 text-sm font-medium {tab.current ? 'border-indigo-500 text-indigo-600' : 'border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700'}"
            >
              <svelte:component
                this={tab.icon}
                aria-hidden="true"
                class="-ml-0.5 mr-2 size-5 {tab.current ? 'text-indigo-500' : 'text-gray-400 group-hover:text-gray-500'}"
              />
              <span>{tab.name}</span>
            </a>
          {/each}
        </nav>
      </div>
    </div>
  </div>
