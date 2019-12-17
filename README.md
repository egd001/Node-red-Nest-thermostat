# NEST style thermostat Dashboard widget for Node-red

If you have made heating or cooling controls on Node-red you will need nice thermostat widget to display and control your device via Node-red dashboard.

![Nest html widget](https://www.ajso.lt/wp-content/uploads/2016/12/nest-html5-widget-1.png)

Fully responsive design. Touch enabled set-point makes it even more cool. Press and hold finger or mouse and it will activate the set point sliding function.

Also it has several display modes like heating, cooling and away. It makes it more interactable and user intuitive. For ECO friendly folks there is possible to turn on and off that little green leaf. 

> Don't forget you have to program your own logics for thermostat functions.

![Nest html widget](https://www.ajso.lt/wp-content/uploads/2016/12/nest-html5-widget_heating-180x180.png)
![Nest html widget](https://www.ajso.lt/wp-content/uploads/2016/12/nest-html5-widget_cooling-180x180.png) 
![Nest html widget](https://www.ajso.lt/wp-content/uploads/2016/12/nest-html5-widget_away-180x180.png)

## How to install:
Open and copy all text from widget.js then go to your node-red application and press **`import`** > **`cliboard`** paste the text and your done.

After import you should see somthing like this:
![Nest html widget](https://www.ajso.lt/wp-content/uploads/2016/12/nest-html5-node-red.png)

## What data can I push to witget

* **`ambient_temperature`** your temperature readings numeric payloads.
* **`target_temperature`** your thermostat setpoint numeric payloads.
* **`hvac_state`** string (off, heating, cooling) payload.
* **`has_leave`** boolean (true, false) payloads.
* **`away`** boolean (true, false) payloads.

If you are familiar with CSS and JAVASSCRIPT there there are more stuff to customize, colors, ranges etc.

## Some options in the script:
```
options = {
       diameter: options.diameter || 400,
       minValue: options.minValue || 10, //Minimum value for target temperature
       maxValue: options.maxValue || 30, //Maximum value for target temperature
       numTicks: options.numTicks || 200, //Number of tick lines to display around the dial
       onSetTargetTemperature: options.onSetTargetTemperature || function() {}, // Function called when new target temperature set by the dial
};
```
## What if I want several widgets on one page?

import several widget just make sure you edit **uniq ID** in these 2 lines inside the UI_Template block and function block that have global variables.

`<div id="Mythermostat1"></div>`

`var nest = new thermostatDial(document.getElementById('Mythermostat1')`

## What if I want  not to round temperature to .5 steps?

You have to change script in these two functions:

```
    /*
    * RENDER - target temperature
    */
    function renderTargetTemperature() {
      lblTarget_text.nodeValue = self.target_temperature;
    	var peggedValue = restrictToRange(self.target_temperature, options.minValue, options.maxValue);
    	degs = properties.tickDegrees * (peggedValue - options.minValue) / properties.rangeValue - properties.offsetDegrees;
    	if (peggedValue > self.ambient_temperature) {
    		degs += 8;
    	} else {
    		degs -= 8;
    	}
    	var pos = rotatePoint(properties.lblTargetPosition, degs, [properties.radius, properties.radius]);
    	attr(lblTarget, {
    		x: pos[0],
    		y: pos[1]
    	});
    }
```

```
    /*
    * RENDER - ambient temperature
    */
    function renderAmbientTemperature() {
    	lblAmbient_text.nodeValue = Math.floor(self.ambient_temperature);
       setClass(lblAmbientHalf, 'shown', true);
    	lblAmbientHalf_text.nodeValue = (self.ambient_temperature % 1).toFixed(1).substring(2);
    }
```


## Data persistance:

Every time you push new vaue to widged values are stored in global context of node-red.

## Issues:

Use the GitHub issues log for raising issues or contributing suggestions and enhancements. [GITHUB](https://github.com/automatikas/Node-red-Nest-thermostat)
