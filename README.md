# TimeSheet
A jQuery plugin for time planning (timetable) with buttons to enable or disable cells

You can use this plugin for planification, planning and a lot of things like parental control and so on.

For full & initial documentation, see [TimeSheet](https://github.com/lbbc1117/TimeSheet)

## Changes from original plugin

### Enable or disable cells with buttons instead of left click and right click
Use ``sheet.setMode(true);`` or ``sheet.setMode(false);`` to set enable mode or disable mode
[![Image from Gyazo](https://i.gyazo.com/a642ca532368f27a997207a9fe542fb7.gif)](https://gyazo.com/a642ca532368f27a997207a9fe542fb7)

### Row selecting
You can select rows in addition to single cell and cols.
[![Image from Gyazo](https://i.gyazo.com/63c293b7d6d4995163fae1607098d89a.gif)](https://gyazo.com/63c293b7d6d4995163fae1607098d89a)

### Clean mode
Use ``sheet.clean(0);`` to disable every cells or ``sheet.clean(1);`` to enable every cells (for example in my case every cells should be enabled by default so that can be useful)

## Demo code
Use this code if you want a planning on 24 hours and 7 days. Useful for planification.

```html
<html lang="fr">
<head>
	<meta charset="utf-8">
	
	<title>Planification</title>

	<!-- Bootstrap core CSS -->
	<link href="http://localhost/planification/assets/bootstrap/css/bootstrap.min.css" rel="stylesheet">
	<link rel="stylesheet" type="text/css" href="http://localhost/planification/assets/utilities/TimeSheet/TimeSheet.css">
</head>
<body id="planification">
	<div class="container py-5">
		<div class="mb-5">
			<a href="#" class="btn btn-success" role="button" id="savePlanification">Enregistrer</a>
			<a href="#" class="btn btn-primary" role="button" id="enablePlanification">Autorisé</a>
			<a href="#" class="btn btn-danger" role="button" id="disablePlanification">Bloqué</a>
		</div>

		<table class="table">
			<thead></thead>
			<tbody id="planning"></tbody>
		</table>
	</div>
	<script>
		var siteUrl = "http://localhost/planification/";
	</script>
	
	<script type="text/javascript" src="http://localhost/planification/assets/utilities/jquery-3.2.1.min.js"></script>
	<script type="text/javascript" src="http://localhost/planification/assets/bootstrap/js/popper.min.js"></script>
	<script type="text/javascript" src="http://localhost/planification/assets/bootstrap/js/bootstrap.min.js"></script>

	<script type="text/javascript" src="http://localhost/planification/assets/utilities/TimeSheet/TimeSheet.js"></script>

	
	<script type="text/javascript">
		var hours = [];
		for(var i = 0; i < 24; i++){
			var hour = i.toString().padStart(2, 0);
			var hourNext = i+1 === 24 ? '00' : (i+1).toString().padStart(2, 0);
			hours.push({name: hour + 'h', title: hour + ':00 - ' + hourNext + ':00'});
		}

		var sheetData = [];
		for(var i = 0; i < 7; i++){
			for(var j = 0; j < 24; j++){
				if(typeof sheetData[i] === "undefined"){
					sheetData[i] = [1];
				}
				else {
					sheetData[i].push(1);
				}
			}
		}

		var sheet = $('#planning').TimeSheet({
			data: {
				dimensions : [7,24],
				colHead : hours,
				rowHead : [{name:"Lundi"},{name:"Mardi"}, {name:"Mercredi"}, {name:"Jeudi"}, {name:"Vendredi"}, {name:"Samedi"}, {name:"Dimanche"}],
				sheetHead : {name:"Planning"},
				sheetData : sheetData
			}
		});

		$('#enablePlanification, #disablePlanification').click(function(e){
			e.preventDefault();

			var mode = $(this).attr('id') === 'enablePlanification' ? true : false;

			sheet.setMode(mode);
		});

		$('#savePlanification').click(function(e){
			var states = sheet.getSheetStates();
			var disabled = [];
			for(var i = 0; i < 7; i++){
				for(var j = 0; j < 24; j++){
					if(states[i][j] === 0) disabled.push({day: i + 1, hour: j, });
				}
			}

			console.log(disabled);
		});
	</script>
</body>
</html>
``
