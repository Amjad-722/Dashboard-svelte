<script>
    import Sidebar from '../component/Sidebar.svelte';
    import Header from '../component/Header.svelte';
    import StatsCard from '../component/StatsCard.svelte';
    import LineChart from '../component/LineChart.svelte';
    import BarChart from '../component/BarChart.svelte';
    import RecentActivity from '../component/RecentActivity.svelte';
    import DataTable from '../component/DataTable.svelte';
    
    // Sample data
    const stats = [
      { title: 'Total Revenue', value: '$45,231.89', change: '+20.1%', trend: 'up' },
      { title: 'New Customers', value: '2,338', change: '+18.2%', trend: 'up' },
      { title: 'Active Sessions', value: '1,893', change: '-5.1%', trend: 'down' },
      { title: 'Conversion Rate', value: '3.24%', change: '+1.2%', trend: 'up' }
    ];
    
    const lineChartData = {
      labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'],
      datasets: [
        {
          values: [30, 40, 35, 50, 49, 60]
        }
      ]
    };
    
    const barChartData = {
      labels: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun'],
      datasets: [
        {
          values: [18, 25, 30, 15, 20, 12, 8]
        }
      ]
    };
    
    const tableData = {
      headers: ['Customer', 'Status', 'Date', 'Amount'],
      rows: [
        { customer: 'John Doe', status: 'Completed', date: 'Apr 23, 2023', amount: '$250.00' },
        { customer: 'Jane Smith', status: 'Processing', date: 'Apr 22, 2023', amount: '$120.50' },
        { customer: 'Robert Johnson', status: 'Completed', date: 'Apr 21, 2023', amount: '$350.00' },
        { customer: 'Emily Davis', status: 'Failed', date: 'Apr 20, 2023', amount: '$75.20' },
        { customer: 'Michael Brown', status: 'Completed', date: 'Apr 19, 2023', amount: '$189.99' }
      ]
    };
    
    const activities = [
      { user: 'John Doe', action: 'created a new project', time: '2 hours ago' },
      { user: 'Jane Smith', action: 'completed a task', time: '4 hours ago' },
      { user: 'Robert Johnson', action: 'uploaded a file', time: '6 hours ago' },
      { user: 'Emily Davis', action: 'commented on a task', time: '1 day ago' },
      { user: 'Michael Brown', action: 'started a new task', time: '1 day ago' }
    ];
  </script>
  
  <div class="flex h-screen bg-gray-100 dark:bg-gray-900">
    <!-- Sidebar -->
    <Sidebar />
    
    <!-- Main Content -->
    <div class="flex-1 flex flex-col overflow-hidden">
      <Header />
      
      <!-- Dashboard Content -->
      <main class="flex-1 overflow-y-auto p-4 md:p-6">
        <div class="max-w-7xl mx-auto">
          <h1 class="text-2xl font-semibold text-gray-800 dark:text-white mb-6">Dashboard</h1>
          
          <!-- Stats Cards -->
          <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mb-6">
            {#each stats as stat}
              <StatsCard 
                title={stat.title} 
                value={stat.value} 
                change={stat.change} 
                trend={stat.trend} 
              />
            {/each}
          </div>
          
          <!-- Charts -->
          <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-6">
            <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-4">
              <h2 class="text-lg font-medium text-gray-700 dark:text-gray-200 mb-4">Revenue Overview</h2>
              <LineChart data={lineChartData} />
            </div>
            <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-4">
              <h2 class="text-lg font-medium text-gray-700 dark:text-gray-200 mb-4">Weekly Sales</h2>
              <BarChart data={barChartData} />
            </div>
          </div>
          
          <!-- Table and Activity -->
          <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
            <div class="lg:col-span-2 bg-white dark:bg-gray-800 rounded-lg shadow">
              <h2 class="text-lg font-medium text-gray-700 dark:text-gray-200 p-4 border-b dark:border-gray-700">Recent Transactions</h2>
              <DataTable data={tableData} />
            </div>
            <div class="bg-white dark:bg-gray-800 rounded-lg shadow">
              <h2 class="text-lg font-medium text-gray-700 dark:text-gray-200 p-4 border-b dark:border-gray-700">Recent Activity</h2>
              <RecentActivity activities={activities} />
            </div>
          </div>
        </div>
      </main>
    </div>
  </div>