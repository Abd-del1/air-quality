<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Air Quality Monitor - India</title>
<script src="https://cdn.tailwindcss.com"></script>
<script>
tailwind.config = {
theme: {
extend: {
colors: {
primary: '#4CAF50',
secondary: '#2196F3'
},
borderRadius: {
'none': '0px',
'sm': '4px',
DEFAULT: '8px',
'md': '12px',
'lg': '16px',
'xl': '20px',
'2xl': '24px',
'3xl': '32px',
'full': '9999px',
'button': '8px'
}
}
}
}
</script>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Pacifico&display=swap" rel="stylesheet">
<link href="https://cdn.jsdelivr.net/npm/remixicon@4.5.0/fonts/remixicon.css" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/echarts/5.5.0/echarts.min.js"></script>
<style>
:where([class^="ri-"])::before { content: "\f3c2"; }
.gauge { transform: rotate(-90deg); }
.gauge circle { transition: stroke-dashoffset 0.3s; }
@keyframes float {
0% { transform: translateY(0px); }
50% { transform: translateY(-20px); }
100% { transform: translateY(0px); }
}
.cloud {
position: absolute;
width: 100px;
height: 40px;
background: #374151;
border-radius: 20px;
box-shadow: 0 8px 16px rgba(0,0,0,0.3);
animation: float 4s ease-in-out infinite;
}
.cloud::before {
content: '';
position: absolute;
width: 50px;
height: 50px;
background: #374151;
border-radius: 50%;
top: -20px;
left: 15px;
}
.cloud::after {
content: '';
position: absolute;
width: 30px;
height: 30px;
background: #374151;
border-radius: 50%;
top: -10px;
left: 45px;
}
.cloud-1 { top: 20%; left: 10%; animation-delay: 0s; }
.cloud-2 { top: 40%; left: 20%; animation-delay: 2s; }
.cloud-3 { top: 25%; left: 30%; animation-delay: 4s; }
</style>
</head>
<body class="bg-gray-900 min-h-screen text-gray-100">
<div class="max-w-7xl mx-auto px-4 py-12 space-y-12 relative overflow-hidden">
<div class="cloud cloud-1"></div>
<div class="cloud cloud-2"></div>
<div class="cloud cloud-3"></div>
<header class="flex justify-between items-center">
<div class="flex items-center gap-6">
<h1 class="text-3xl font-bold bg-gradient-to-r from-primary to-secondary bg-clip-text text-transparent">Air Quality Monitor</h1>
<div class="relative">
<button id="locationBtn" class="flex items-center gap-2 px-4 py-2 bg-gray-800 rounded-button shadow-sm hover:bg-gray-700 transition-all duration-200 border border-gray-700">
<i class="ri-map-pin-line w-5 h-5 flex items-center justify-center"></i>
<span id="selectedLocation">New Delhi</span>
<i class="ri-arrow-down-s-line w-5 h-5 flex items-center justify-center"></i>
</button>
<div id="locationDropdown" class="hidden absolute top-full left-0 mt-2 w-48 bg-gray-800 rounded shadow-lg z-10">
<div class="py-1">
<a href="#" class="block px-4 py-2 hover:bg-gray-700" onclick="changeLocation('New Delhi')">New Delhi</a>
<a href="#" class="block px-4 py-2 hover:bg-gray-700" onclick="changeLocation('Mumbai')">Mumbai</a>
<a href="#" class="block px-4 py-2 hover:bg-gray-700" onclick="changeLocation('Bangalore')">Bangalore</a>
<a href="#" class="block px-4 py-2 hover:bg-gray-700" onclick="changeLocation('Chennai')">Chennai</a>
</div>
</div>
</div>
</div>
<div class="flex items-center gap-4">
<div class="text-gray-600">
<span id="currentDate"></span>
<span id="currentTime"></span>
</div>
<button onclick="refreshData()" class="p-2 hover:bg-gray-100 rounded-full">
<i class="ri-refresh-line w-6 h-6 flex items-center justify-center"></i>
</button>
</div>
</header>
<div class="grid grid-cols-1 lg:grid-cols-3 gap-8">
<div class="bg-gray-800 p-8 rounded-xl shadow-sm hover:shadow-md transition-shadow duration-300 border border-gray-700">
<div class="text-center">
<div class="relative w-48 h-48 mx-auto mb-4">
<svg class="gauge" viewBox="0 0 120 120" width="100%" height="100%">
<circle cx="60" cy="60" r="54" fill="none" stroke="#e6e6e6" stroke-width="12"/>
<circle id="aqi-gauge" cx="60" cy="60" r="54" fill="none" stroke="#4CAF50" stroke-width="12" stroke-dasharray="339.292" stroke-dashoffset="0"/>
</svg>
<div class="absolute inset-0 flex flex-col items-center justify-center">
<span id="aqi-value" class="text-4xl font-bold">75</span>
<span id="aqi-status" class="text-lg text-gray-600">Moderate</span>
</div>
</div>
<div class="flex justify-center gap-8">
<div>
<i class="ri-temp-hot-line w-6 h-6 flex items-center justify-center text-gray-600"></i>
<span id="temperature">28°C</span>
</div>
<div>
<i class="ri-drop-line w-6 h-6 flex items-center justify-center text-gray-600"></i>
<span id="humidity">65%</span>
</div>
</div>
</div>
</div>
<div class="bg-gray-800 p-8 rounded-xl shadow-sm hover:shadow-md transition-shadow duration-300 border border-gray-700">
<h2 class="text-lg font-semibold mb-4">Pollutants</h2>
<div class="grid grid-cols-2 gap-4">
<div class="pollutant-card p-4 border border-gray-700 rounded-lg hover:border-primary/30 transition-colors duration-200">
<div class="flex items-center gap-2 mb-2">
<i class="ri-cloud-line w-5 h-5 flex items-center justify-center text-gray-600"></i>
<span>PM2.5</span>
</div>
<div class="text-2xl font-semibold mb-2">35 µg/m³</div>
<div class="h-2 bg-gray-100 rounded-full overflow-hidden">
<div class="h-full bg-gradient-to-r from-primary to-primary/80" style="width: 35%"></div>
</div>
</div>
<div class="pollutant-card p-4 border border-gray-700 rounded-lg hover:border-primary/30 transition-colors duration-200">
<div class="flex items-center gap-2 mb-2">
<i class="ri-cloud-line w-5 h-5 flex items-center justify-center text-gray-600"></i>
<span>PM10</span>
</div>
<div class="text-2xl font-semibold mb-2">75 µg/m³</div>
<div class="h-2 bg-gray-200 rounded-full overflow-hidden">
<div class="h-full bg-primary" style="width: 50%"></div>
</div>
</div>
<div class="pollutant-card p-4 border border-gray-700 rounded-lg hover:border-primary/30 transition-colors duration-200">
<div class="flex items-center gap-2 mb-2">
<i class="ri-cloud-line w-5 h-5 flex items-center justify-center text-gray-600"></i>
<span>NO₂</span>
</div>
<div class="text-2xl font-semibold mb-2">45 ppb</div>
<div class="h-2 bg-gray-200 rounded-full overflow-hidden">
<div class="h-full bg-primary" style="width: 30%"></div>
</div>
</div>
<div class="pollutant-card p-4 border border-gray-700 rounded-lg hover:border-primary/30 transition-colors duration-200">
<div class="flex items-center gap-2 mb-2">
<i class="ri-cloud-line w-5 h-5 flex items-center justify-center text-gray-600"></i>
<span>O₃</span>
</div>
<div class="text-2xl font-semibold mb-2">28 ppb</div>
<div class="h-2 bg-gray-200 rounded-full overflow-hidden">
<div class="h-full bg-primary" style="width: 20%"></div>
</div>
</div>
</div>
</div>
<div class="bg-gray-800 p-8 rounded-xl shadow-sm hover:shadow-md transition-shadow duration-300 border border-gray-700">
<h2 class="text-lg font-semibold mb-4">Health Advisory</h2>
<div class="p-6 bg-gradient-to-r from-yellow-900/30 to-orange-900/30 rounded-xl mb-6 border border-yellow-700">
<div class="flex items-center gap-2 mb-2">
<i class="ri-alert-line w-5 h-5 flex items-center justify-center text-yellow-400"></i>
<span class="font-medium text-yellow-200">Moderate Health Concern</span>
</div>
<p class="text-sm text-yellow-300">Air quality is acceptable; however, there may be a moderate health concern for a very small number of people who are unusually sensitive to air pollution.</p>
</div>
<div class="space-y-4">
<div>
<h3 class="font-medium mb-2">Sensitive Groups:</h3>
<ul class="text-sm text-gray-600 space-y-1">
<li>• People with respiratory diseases</li>
<li>• Children and elderly</li>
<li>• Outdoor workers</li>
</ul>
</div>
<div>
<h3 class="font-medium mb-2">Recommendations:</h3>
<ul class="text-sm text-gray-600 space-y-1">
<li>• Consider reducing prolonged outdoor activities</li>
<li>• Keep windows closed during peak pollution hours</li>
<li>• Use air purifiers indoors</li>
</ul>
</div>
</div>
</div>
</div>
<div class="grid grid-cols-1 lg:grid-cols-2 gap-8 mb-8">
<div class="bg-gray-800 p-8 rounded-xl shadow-sm hover:shadow-md transition-shadow duration-300 border border-gray-700">
<h2 class="text-lg font-semibold mb-4">24-Hour Forecast</h2>
<div id="forecastChart" class="h-64"></div>
</div>
<div class="bg-gray-800 p-8 rounded-xl shadow-sm hover:shadow-md transition-shadow duration-300 border border-gray-700">
<h2 class="text-lg font-semibold mb-4">Regional Comparison</h2>
<div class="relative h-64 rounded overflow-hidden mb-4">
<img src="https://public.readdy.ai/gen_page/map_placeholder_1280x720.png" alt="Map" class="w-full h-full object-cover">
<div class="absolute top-4 left-4 bg-white px-3 py-2 rounded shadow-sm">
<div class="flex items-center gap-2">
<span class="w-3 h-3 rounded-full bg-green-500"></span>
<span class="text-sm">Good</span>
</div>
<div class="flex items-center gap-2">
<span class="w-3 h-3 rounded-full bg-yellow-500"></span>
<span class="text-sm">Moderate</span>
</div>
<div class="flex items-center gap-2">
<span class="w-3 h-3 rounded-full bg-red-500"></span>
<span class="text-sm">Poor</span>
</div>
</div>
</div>
</div>
</div>
<footer class="flex justify-between items-center text-sm text-gray-400 pt-8 border-t border-gray-700">
<div>
Data source: Central Pollution Control Board (CPCB)
</div>
<div class="flex items-center gap-4">
<button class="flex items-center gap-2 px-4 py-2 rounded-full hover:bg-gray-700 transition-colors duration-200">
<i class="ri-settings-3-line w-5 h-5 flex items-center justify-center"></i>
Settings
</button>
<button class="flex items-center gap-2 px-4 py-2 rounded-full hover:bg-gray-700 transition-colors duration-200">
<i class="ri-question-line w-5 h-5 flex items-center justify-center"></i>
Help
</button>
</div>
</footer>
</div>
<script>
const mockData = {
locations: {
'New Delhi': { aqi: 75, temp: 28, humidity: 65 },
'Mumbai': { aqi: 55, temp: 30, humidity: 75 },
'Bangalore': { aqi: 45, temp: 25, humidity: 60 },
'Chennai': { aqi: 65, temp: 32, humidity: 70 }
}
};
function updateDateTime() {
const now = new Date();
document.getElementById('currentDate').textContent = now.toLocaleDateString('en-US', {
weekday: 'long',
year: 'numeric',
month: 'long',
day: 'numeric'
});
document.getElementById('currentTime').textContent = now.toLocaleTimeString('en-US', {
hour: '2-digit',
minute: '2-digit'
});
}
function initForecastChart() {
const chart = echarts.init(document.getElementById('forecastChart'));
const hours = Array.from({length: 24}, (_, i) => `${i}:00`);
const data = Array.from({length: 24}, () => Math.floor(Math.random() * 50) + 50);
const option = {
animation: false,
tooltip: {
trigger: 'axis',
backgroundColor: 'rgba(255, 255, 255, 0.9)'
},
xAxis: {
type: 'category',
data: hours,
axisLine: { show: false },
axisTick: { show: false }
},
yAxis: {
type: 'value',
axisLine: { show: false },
axisTick: { show: false }
},
series: [{
data: data,
type: 'line',
smooth: true,
symbol: 'none',
lineStyle: { color: '#4CAF50' },
areaStyle: {
color: {
type: 'linear',
x: 0, y: 0, x2: 0, y2: 1,
colorStops: [{
offset: 0,
color: 'rgba(76, 175, 80, 0.2)'
}, {
offset: 1,
color: 'rgba(76, 175, 80, 0)'
}]
}
}
}],
grid: { top: 20, right: 20, bottom: 20, left: 40 }
};
chart.setOption(option);
}
function updateAQIGauge(value) {
const circle = document.getElementById('aqi-gauge');
const circumference = 2 * Math.PI * 54;
const offset = circumference - (value / 500) * circumference;
circle.style.strokeDasharray = `${circumference} ${circumference}`;
circle.style.strokeDashoffset = offset;
let color = '#4CAF50';
let status = 'Good';
if (value > 300) {
color = '#D32F2F';
status = 'Hazardous';
} else if (value > 200) {
color = '#F44336';
status = 'Very Unhealthy';
} else if (value > 150) {
color = '#FF9800';
status = 'Unhealthy';
} else if (value > 100) {
color = '#FFC107';
status = 'Moderate';
} else if (value > 50) {
color = '#8BC34A';
status = 'Good';
}
circle.style.stroke = color;
document.getElementById('aqi-value').textContent = value;
document.getElementById('aqi-status').textContent = status;
}
document.getElementById('locationBtn').addEventListener('click', function() {
const dropdown = document.getElementById('locationDropdown');
dropdown.classList.toggle('hidden');
});
function changeLocation(location) {
document.getElementById('selectedLocation').textContent = location;
document.getElementById('locationDropdown').classList.add('hidden');
const locationData = mockData.locations[location];
if (locationData) {
updateAQIGauge(locationData.aqi);
document.getElementById('temperature').textContent = `${locationData.temp}°C`;
document.getElementById('humidity').textContent = `${locationData.humidity}%`;
}
}
function refreshData() {
const currentLocation = document.getElementById('selectedLocation').textContent;
changeLocation(currentLocation);
}
document.addEventListener('click', function(event) {
const dropdown = document.getElementById('locationDropdown');
const locationBtn = document.getElementById('locationBtn');
if (!dropdown.contains(event.target) && !locationBtn.contains(event.target)) {
dropdown.classList.add('hidden');
}
});
updateDateTime();
setInterval(updateDateTime, 60000);
initForecastChart();
updateAQIGauge(75);
window.addEventListener('resize', function() {
const chart = echarts.getInstanceByDom(document.getElementById('forecastChart'));
if (chart) {
chart.resize();
}
});
</script>
</body>
</html>
