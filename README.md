
<p align="center">
  <a href="http://luisvinicius167.github.io/riotux/"><img src ="https://files.slack.com/files-pri/T02QC0DMD-F0WPW57TJ/riotux_logo.png?pub_secret=d695cfd8bd" alt="riotux event controller inspired in Flux Pattern." width="300" style="max-width:100%;"/></a>
</p>

## Intro 
Simple Event Controller for Riot.js, inspired in Flux Pattern. **riotux** provides more organization for datastore in your application. Your Stores can call and listen events to other Stores and Components, that can call and listen events to Stores too. The Dispatcher is used to communicate between Components.

See the <a href="http://luisvinicius167.github.io/riotux">Demo.</a>

### Install
Requires Riot 2.0+
##### Npm:
``` npm install riotux ```

##### Bower:
``` bower install riotux ```

### Manual install
Just include ``` dist/riotux.min.js``` file in your project.


### Examples:

<a href="http://luisvinicius167.github.io/riot-riotux-blog">Blog example with Riot.js and riotux</a>

### Stores: 
The Stores are a riot.observable(). Stores can listen and trigger methods for other Stores and Components. The method will applied for the Store that you passing in argument like 'storeName'. If you have two methods with the same name, the method that will be call is the method to the store that have the same name that you passed in arguments.

Stores Data-Flow example:
```javascript
// Your Car Store
function CarStore ( ) {
  riot.observable(this);
  
  // listen to 'start' event
  this.on('start', function ( person ) {
    console.log(person + ' started the Car.')
    // Emmit the method for view that listen to
    this.trigger('carMoving', person);
  });
};

var carStore = new CarStore();
riotux.addStore('carStore', carStore);
```

```javascript
// Your Other Store
function PersonStore ( ) {
  riot.observable(this);
 
  // listen to 'startCar' event
  this.on('startCar', function ( person ) {
    riotux.trigger('carStore', 'start', person);
  });
};

var personStore = new PersonStore();
riotux.addStore('personStore', personStore);
```

```javascript
/**----------------------------------- 
  * Data-Flow: Component -> personStore -> carStore -> Component.
  *-----------------------------------
  * When the component is mounted, trigger the event 'startCar' to 
  * personStore, passing 'You' like argument.
  * The personStore trigger the event to carStore passing the argument too.
  * carStore recieves and trigger for the components that listen to event.
  */

// In your component .tag
this.on('mount', function ( ) {
  riotux.trigger('personStore', 'startCar', 'You');
});

// listen the method from carStore
riotux.on('carStore', 'carMoving', function (person) {
  console.log(person + ' started the Car.');
});

// > output: You started the Car.
```

### Dispatcher
The Dispatcher connects your Component with other Components. If you need to listen a method present in one Component inside your Component, you need register this using the method ```riotux.listen```, that will register your method inside the Dispatcher. You can use the method```riotux.emmit``` passing the event name for the method that you want to call to other Component that listen to.

Dispatcher Data-Flow example in View:

```html
<!-- In your .tag component -->

<script>
    var self = this; 
    self.status = false;
    
    self.on('mount', function(){
      riotux.emmit('eventName', 'Argument');  
    });
</script>
```

```html
<!-- In your other .tag component -->

<script>
    var self = this; 
    self.status = false;
    
    riotux.listen('eventName', function ( arg ) {
      console.log(arg);
    });

</script>
// > output: Argument.
```

### API
All stores should be created and registered before the Riot app is mounted to works fine.

Add an Store:
 * ```riotux.addStore(storeName, Store)```
 
**Stores**:
 
 * ```riotux.on(storeName, event, callback)```: listen the event for the Store that pass like storeName.
 
 * ```riotux.trigger(storeName, event [, args])```: trigger the event for the Store that pass like storeName. If more one Store listen the method, you need to pass the storeNames inside an Array.
 
 * ```riotux.one(storeName, event, callback)```: listen the event, just one time, for the Store that pass like storeName.
 
 * ```riotux.off(storeName, event [, callback])```: Cancel the event for the Store that pass like storeName.


**Dispatcher**:
 
 * ```riotux.listen(event, callback)```: listen an event 
 
 * ```riotux.emmit(event [, args])```: trigger an event. 
 
 * ```riotux.listenOne(event, callback)```: listen an event, just one time. 
 
 * ```riotux.cancel(event [, callback])```: remove an event. 

**Helper methods**:
 
 * ```riotux.getDispatcherEvents( eventAPI )```: return the events initialized in Dispatcher at the momen, eventAPI can be: ```listen```, ```listenOne```, ```emmit``` and ```cancel```.

### License
MIT License.
