---
layout: tutorial_frame
title: Zoom Delta Tutorial
---
<script>

	var mad = L.latLng([40.40, -3.7]);

	var map = L.map('map', {
		minZoom: 3,
		maxZoom: 6
	});

	var cartodbAttribution = '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors, &copy; <a href="http://cartodb.com/attributions">CartoDB</a>';

	var positron = L.tileLayer('http://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}.png', {
		attribution: cartodbAttribution,
		minZoom: 4,
		maxZoom: 5
	}).addTo(map);


	var timeouts = [];

	function zoomCycle(){
		map.setView(mad,3);
		timeouts = [];
		timeouts.push(setTimeout(function(){ map.setView(mad, 4); },  2000));
		timeouts.push(setTimeout(function(){ map.setView(mad, 5); },  4000));
		timeouts.push(setTimeout(function(){ map.setView(mad, 6); },  6000));
		timeouts.push(setTimeout(function(){ map.setView(mad, 5); },  8000));
		timeouts.push(setTimeout(function(){ map.setView(mad, 4); }, 10000));
	}
	zoomCycle();

	var zoomingInterval = setInterval(zoomCycle, 12000);

	var ZoomViewer = L.Control.extend({
		onAdd: function(){

			var container= L.DomUtil.create('div');
			var gauge = L.DomUtil.create('div');
			container.style.width = '200px';
			container.style.background = 'rgba(255,255,255,0.5)';
			container.style.textAlign = 'left';
			map.on('zoomstart zoom zoomend', function(ev){
				gauge.innerHTML = 'Zoom level: ' + map.getZoom();
			})
			container.appendChild(gauge);
			var stopButton = L.DomUtil.create('button');
			stopButton.innerHTML = 'Stop auto-zooming';
			L.DomEvent.on(stopButton, 'click', function(){
				clearInterval(zoomingInterval);
				for (var i in timeouts) { clearTimeout(timeouts[i]); }
			});
			container.appendChild(stopButton);
			return container;
		}
	});

	(new ZoomViewer).addTo(map);

</script>
