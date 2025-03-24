<script>
    import { onMount } from 'svelte';
    import { browser } from '$app/environment';
    
    export let data = {
      labels: [],
      datasets: [{ values: [] }]
    };
    
    let canvas;
    let chart;
    
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
      const xStep = (width - padding * 2) / (data.labels.length - 1);
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
        const x = padding + i * xStep;
        ctx.fillText(label, x, height - padding + labelPadding + 10);
      });
      
      // Draw line
      ctx.strokeStyle = '#4f46e5';
      ctx.lineWidth = 3;
      ctx.lineJoin = 'round';
      ctx.beginPath();
      data.datasets[0].values.forEach((value, i) => {
        const x = padding + i * xStep;
        const y = height - padding - (value * yStep);
        if (i === 0) {
          ctx.moveTo(x, y);
        } else {
          ctx.lineTo(x, y);
        }
      });
      ctx.stroke();
      
      // Draw points
      ctx.fillStyle = '#4f46e5';
      data.datasets[0].values.forEach((value, i) => {
        const x = padding + i * xStep;
        const y = height - padding - (value * yStep);
        ctx.beginPath();
        ctx.arc(x, y, 5, 0, Math.PI * 2);
        ctx.fill();
        
        // Draw white inner circle
        ctx.fillStyle = isDarkMode ? '#1f2937' : 'white';
        ctx.beginPath();
        ctx.arc(x, y, 2, 0, Math.PI * 2);
        ctx.fill();
        ctx.fillStyle = '#4f46e5';
      });
      
      // Draw area under the line
      const gradient = ctx.createLinearGradient(0, 0, 0, height);
      gradient.addColorStop(0, 'rgba(79, 70, 229, 0.4)');
      gradient.addColorStop(1, 'rgba(79, 70, 229, 0)');
      
      ctx.fillStyle = gradient;
      ctx.beginPath();
      ctx.moveTo(padding, height - padding);
      data.datasets[0].values.forEach((value, i) => {
        const x = padding + i * xStep;
        const y = height - padding - (value * yStep);
        ctx.lineTo(x, y);
      });
      ctx.lineTo(width - padding, height - padding);
      ctx.closePath();
      ctx.fill();
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