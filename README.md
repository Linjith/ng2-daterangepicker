## ng2-daterangepicker-ext
This is an Angular 2+ port of the popular Date Range Picker for Bootstrap http://www.daterangepicker.com/

And forke of https://github.com/evansmwendwa/ng2-daterangepicker

### Installation
```
npm install ng2-daterangepicker-ext --save
```

### SystemJS Mapping
If you are using a SystemJS setup then map `ng2-daterangepicker-ext` and it's `dependencies` in your `system.config.js` file

``` javascript
paths: {
    'npm:': 'node_modules/'
},
map: {
    'ng2-daterangepicker-ext': 'npm:ng2-daterangepicker-ext',
    'jquery': 'npm:jquery/dist/jquery.js',
    'moment': 'npm:moment',
    'bootstrap-daterangepicker-ext': 'npm:bootstrap-daterangepicker-ext/daterangepicker.js'
},
packages: {

    moment: {
        main: 'moment',
        defaultExtension: 'js'
    },
    'ng2-daterangepicker': {
        main: 'index',
        defaultExtension: 'js'
     }
}
```

### import Daterangepicker Module
import the `Daterangepicker` module in your application module

``` javascript
import { Daterangepicker } from 'ng2-daterangepicker-ext';

@NgModule({
    imports: [Daterangepicker]
})

```

Use the `daterangepicker` directive in your component by passing in options `{}` and consuming the `selected` event. Directive can be added to inputs, buttons or any other html element

``` javascript
@Component({
    selector: 'my-app',
    template: `<input type="text" name="daterangeInput" daterangepicker [options]="options" (selected)="selectedDate($event, daterange)" />`,
})
export class AppComponent {

    public daterange: any = {};

    // see original project for full list of options
    // can also be setup using the config service to apply to multiple pickers
    public options: any = {
        locale: { format: 'YYYY-MM-DD' },
        alwaysShowCalendars: false,
    };

    public selectedDate(value: any, datepicker?: any) {
        console.log(value);
        datepicker.start = value.start;
        datepicker.end = value.end;
        this.daterange.start = value.start;
        this.daterange.end = value.end;
        this.daterange.label = value.label;
    }
}
```

You can pass global settings that can be overloaded by the `options` object in the daterangepicker instances. Use the `DaterangepickerConfig` service to do so. The service provider is set in the daterangepicker module.

``` javascript
import { DaterangepickerConfig } from 'ng2-daterangepicker-ext';

@Component({
    selector:'my-app',
    template:'<h3>Component Template</h3>'
})
export class AppComponent {

    constructor(private daterangepickerOptions: DaterangepickerConfig) {
        this.daterangepickerOptions.settings = {
            locale: { format: 'YYYY-MM-DD' },
            alwaysShowCalendars: false
        };
    }
}
```

## Customizing the CSS
> **Skip Adding CSS in Head** - You can bundle the css that styles the calendar in your app by copying or importing the contents of `node_modules/ng2-daterangepicker-ext/daterangepicker.component.css` and customizing as you like. When you do so set the `skipCSS` property of the config service to `true` in order for the module to skip adding the css styles in the head.

> **NB:** Skipping should be done early in your component where the service is used first and before your component has rendered.

> **NB:** Note that the picker dropdown might be added in the body element outside of your component so the css must not be encapsulated by the component. The styles should be added to your global styles.

``` javascript
import { DaterangepickerConfig } from 'ng2-daterangepicker-ext';

@Component({
    selector:'my-app',
    template:'<h3>Component Template</h3>'
})
export class AppComponent {

    constructor(private daterangepickerOptions: DaterangepickerConfig) {
        this.daterangepickerOptions.skipCSS = true;
    }
}
```

## Daterangepicker methods

You can programmatically update the `startDate` and `endDate` in the picker using the `setStartDate` and `setEndDate` methods. You can access the Date Range Picker object and its functions and properties through the `datePicker` property of the directive using `@ViewChild`.

``` javascript
import { Component, AfterViewInit, ViewChild  } from '@angular/core';
import { DaterangePickerComponent } from 'ng2-daterangepicker-ext';

@Component({
    selector:'my-app',
    template:'<h3>Component Template</h3>'
})
export class AppComponent {

    @ViewChild(DaterangePickerComponent)
    private picker: DaterangePickerComponent;

    public updateDateRange() {
        this.picker.datePicker.setStartDate('2017-03-27');
        this.picker.datePicker.setEndDate('2017-04-08');
    }
}
```

## Using Daterangepicker Events

You can bind to the events fired by the daterangepicker. All events will emit an object containing the event fired and the datepicker object.

```
{
    event: {},
    picker: {}
}
```

#### Available events

Below is the list of events that you can bind into.

Visit the original site for detailed options and documentation http://www.daterangepicker.com/#options

```
cancelDaterangepicker
applyDaterangepicker
hideCalendarDaterangepicker
showCalendarDaterangepicker
hideDaterangepicker
showDaterangepicker
clickSelectDaterangePicker - gives the selectedDate
```

Below is a sample usage. You can have multiple methods and implement only the events you want.

Create methods that will be called by the events in your component and bind to fired events in the component's template.

``` javascript
@Component({
    selector: 'my-app',
    template: `<input type="text" name="daterangeInput" daterangepicker [options]="options" (selected)="selectedDate($event)"
    (cancelDaterangepicker)="calendarCanceled($event)"
    (applyDaterangepicker)="calendarApplied($event)"
    />`,
})
export class AppComponent {

    public daterange: any = {};

    private selectedDate(value: any) {
        daterange.start = value.start;
        daterange.end = value.end;
    }

    // expected output is an object containing the event and the picker.
    // you can add multiple params to the method and pass them in the template
    public calendarCanceled(e:any) {
        console.log(e);
    }

    public calendarApplied(e:any) {
        console.log(e);
    }
}
```

Below is the link to the original project, there's more info regarding the daterangepicker there. http://www.daterangepicker.com/

> **Note:**
* This component is in active development so keep checking for new releases and update to the latest version.
* This Component depends on jQuery, [Moment.js](http://momentjs.com/) and Bootstrap to work correctly.
* I have recently added support for `SystemJS` builds in a minor version with a lot of build changes but I do not expect anything to break, if you encounter errors please post an issue or do a pull request to patch it.
* Contribution is highly encouraged.
* This package is an Angular (2+) port. see original project http://www.daterangepicker.com
