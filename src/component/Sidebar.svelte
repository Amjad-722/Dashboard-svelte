<script>
    import { page } from '$app/stores';
    import { 
      Home, 
      BarChart2, 
      Users, 
      ShoppingCart, 
      FileText, 
      Settings, 
      HelpCircle,
      Menu,
      X
    } from 'lucide-svelte';
    
    let isMobileMenuOpen = false;
    
    const toggleMobileMenu = () => {
      isMobileMenuOpen = !isMobileMenuOpen;
    };
    
    const navItems = [
      { icon: Home, label: 'Dashboard', href: '/' },
      { icon: BarChart2, label: 'Analytics', href: '/analytics' },
      { icon: Users, label: 'Customers', href: '/customers' },
      { icon: ShoppingCart, label: 'Orders', href: '/orders' },
      { icon: FileText, label: 'Reports', href: '/reports' },
      { icon: Settings, label: 'Settings', href: '/settings' },
      { icon: HelpCircle, label: 'Help', href: '/help' }
    ];
  </script>
  
  <!-- Mobile menu button -->
  <button 
    class="md:hidden fixed top-4 left-4 z-50 p-2 rounded-md bg-gray-800 text-white"
    on:click={toggleMobileMenu}
    aria-label={isMobileMenuOpen ? 'Close menu' : 'Open menu'}
  >
    {#if isMobileMenuOpen}
      <X size={20} />
    {:else}
      <Menu size={20} />
    {/if}
  </button>
  
  <!-- Sidebar for desktop -->
  <aside class="hidden md:flex md:w-64 flex-col bg-gray-800 text-white">
    <div class="p-4 border-b border-gray-700">
      <div class="flex items-center">
        <div class="w-8 h-8 rounded-md bg-indigo-600 flex items-center justify-center mr-2">
          <span class="font-bold">D</span>
        </div>
        <span class="text-xl font-semibold">Dashboard</span>
      </div>
    </div>
    
    <nav class="flex-1 p-4">
      <ul class="space-y-1">
        {#each navItems as item}
          <li>
            <a 
              href={item.href} 
              class="flex items-center px-4 py-2 rounded-md hover:bg-gray-700 transition-colors {$page.url.pathname === item.href ? 'bg-gray-700' : ''}"
            >
              <svelte:component this={item.icon} size={20} class="mr-3" />
              <span>{item.label}</span>
            </a>
          </li>
        {/each}
      </ul>
    </nav>
    
    <div class="p-4 border-t border-gray-700">
      <div class="flex items-center">
        <div class="w-8 h-8 rounded-full bg-gray-600 mr-2"></div>
        <div>
          <p class="text-sm font-medium">John Doe</p>
          <p class="text-xs text-gray-400">Administrator</p>
        </div>
      </div>
    </div>
  </aside>
  
  <!-- Mobile sidebar -->
  <aside 
    class="md:hidden fixed inset-0 z-40 bg-gray-800 text-white transform {isMobileMenuOpen ? 'translate-x-0' : '-translate-x-full'} transition-transform duration-300 ease-in-out"
  >
    <div class="p-4 border-b border-gray-700">
      <div class="flex items-center justify-between">
        <div class="flex items-center">
          <div class="w-8 h-8 rounded-md bg-indigo-600 flex items-center justify-center mr-2">
            <span class="font-bold">D</span>
          </div>
          <span class="text-xl font-semibold">Dashboard</span>
        </div>
        <button on:click={toggleMobileMenu} class="p-2">
          <X size={20} />
        </button>
      </div>
    </div>
    
    <nav class="p-4">
      <ul class="space-y-1">
        {#each navItems as item}
          <li>
            <a 
              href={item.href} 
              class="flex items-center px-4 py-2 rounded-md hover:bg-gray-700 transition-colors {$page.url.pathname === item.href ? 'bg-gray-700' : ''}"
              on:click={() => isMobileMenuOpen = false}
            >
              <svelte:component this={item.icon} size={20} class="mr-3" />
              <span>{item.label}</span>
            </a>
          </li>
        {/each}
      </ul>
    </nav>
    
    <div class="p-4 border-t border-gray-700 absolute bottom-0 w-full">
      <div class="flex items-center">
        <div class="w-8 h-8 rounded-full bg-gray-600 mr-2"></div>
        <div>
          <p class="text-sm font-medium">John Doe</p>
          <p class="text-xs text-gray-400">Administrator</p>
        </div>
      </div>
    </div>
  </aside>