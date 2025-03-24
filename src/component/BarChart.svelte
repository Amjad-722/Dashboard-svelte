<script>
    import { onMount } from 'svelte';
    import { browser } from '$app/environment';
    
    export let data = {
      labels: [],
      datasets: [{ values: [] }]
    };
    
    let canvas;
    
    onMount(() => {
      if (browser) {
        drawChart();
        
        // Redraw on theme change
        const observer = new MutationObserver((mutations) => {
          mutations.forEach((mutation) => {
            if (mutation.attributeName === 'class') {
              drawChart();
            }
          });
        });
        
        observer.observe(document.documentElement, { attributes: true });
        
        return () => {
          observer.disconnect();
        };
      }
    });
    
    function drawChart() {
      if (!canvas) return;
      
      const ctx = canvas.getContext('2d');
      const isDarkMode = document.documentElement.classList.contains('dark');
      
      // Clear previous drawing
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      
      const width = canvas.width;
      const height = canvas.height;
      const padding = 40;
      const labelPadding = 10;
      
      // Calculate scales
      const maxValue = Math.max(...data.datasets[0].values) * 1.1;
      const barWidth = (width - padding * 2) / data.labels.length * 0.6;
      const barSpacing = (width - padding * 2) / data.labels.length;
      const yStep = (height - padding * 2) / maxValue;
      
      // Draw grid lines
      ctx.strokeStyle = isDarkMode ? 'rgba(255, 255, 255, 0.1)' : 'rgba(0, 0, 0, 0.1)';
      ctx.lineWidth = 1;
      
      // Horizontal grid lines
      for (let i = 0; i <= 5; i++) {
        const y = height - padding - (i * (height - padding * 2) / 5);
        ctx.beginPath();
        ctx.moveTo(padding, y);
        ctx.lineTo(width - padding, y);
        ctx.stroke();
        
        // Y-axis labels
        ctx.fillStyle = isDarkMode ? 'rgba(255, 255, 255, 0.7)' : 'rgba(0, 0, 0, 0.7)';
        ctx.font = '12px Arial';
        ctx.textAlign = 'right';
        ctx.fillText(Math.round(maxValue * i / 5).toString(), padding - labelPadding, y + 4);
      }
      
      // Draw x-axis labels
      ctx.fillStyle = isDarkMode ? 'rgba(255, 255, 255, 0.7)' : 'rgba(0, 0, 0, 0.7)';
      ctx.font = '12px Arial';
      ctx.textAlign = 'center';
      data.labels.forEach((label, i) => {
        const x = padding + i * barSpacing + barWidth / 2;
        ctx.fillText(label, x, height - padding + labelPadding + 10);
      });
      
      // Draw bars
      data.datasets[0].values.forEach((value, i) => {
        const x = padding + i * barSpacing;
        const barHeight = value * yStep;
        const y = height - padding - barHeight;
        
        // Create gradient for bar
        const gradient = ctx.createLinearGradient(0, y, 0, height - padding);
        gradient.addColorStop(0, '#4f46e5');
        gradient.addColorStop(1, '#818cf8');
        
        ctx.fillStyle = gradient;
        ctx.beginPath();
        ctx.roundRect(x, y, barWidth, barHeight, [4, 4, 0, 0]);
        ctx.fill();
      });
    }
  </script>
  
  <div class="w-full h-64">
    <canvas 
      bind:this={canvas} 
      width={800} 
      height={400} 
      class="w-full h-full"
    ></canvas>
  </div>