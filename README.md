# Riotux
Easy Event Controller for Riot.js based in Namespaces and Stores.
Store can listen and trigger events for other Stores and Views. Views also listen and trigger events for other Views and Stores too. 

#####``` Store -> View | View  -> View | Store -> Store ```

## Install
``` npm install riotux ```

## Manual install
Just include ``` riotux.min.js``` file in your project.

## Usage 
Requires Riot 2.0+

## API
Riotux provides an Easy way to Dispatch events, using riot.observable like global dispaticher. 
Riotux is based on Namespaces/Observers and Stores. In the Stores you can register the methods and organize your namespaces according a store. All events is based following this structure: ``` 'namespace:event' ```

* ``` addStore ```: This method add an Store with this namespace.
* ``` getStores ```: Return all Stores who you have in your application
* ``` removeStores ```: Remove all Stores
* ``` getNamespace( namespace ) ```: Return all methods with the namespace who you pass in the argument.
* ``` getNamespaces() ```: Return an array with all namespaces you have


### Store
Is a Function. You need to add the store using the method addStore, who recieves two arguments: the store name and the store.
Method: ``` Riotux.addStore(storeName, store) ```

```javascript

// Example
function ProductStore () {
  // Product is the namespace of ProductStore
  Riotux.on('Product:start', function ( name ) {
    console.log('Initied Product Store by ' + name );
  })
}
var ProductStore = new ProductStore;
Riotux.addStore('Product', ProductStore);

```
#### Trigger an Event
Just trigger the event fot the listenrs who have the namespace
``` Riotux.trigger('namespace:event', args) ```

### View
In the Riot tag View you can trigger and listen events. Listen for event, and execute callback when it is triggered. This applies just for the view or store who contains this namespace, so that you may receive the same event from multiple sources who contais this namespace.

```javascript

// Example, inside a Riot View (tag):
Riotux.trigger('Products:start', 'Luis Vinicius')
Riotux.on(event, callback)

// Example, inside Riot view (tag):
Riotux.on('Product:start', function ( ) {
    console.log('Started');
})
``` 

####Remove event listener.

```javascript
  
  Riotux.off(event);
  Riotux.off(event, callback);
```

Same as Riotux.on(), executes once.
``` Riotux.one(event, callback) ```

#### License
MIT License.

#### Thaks to
RiotControl for the inspiration.