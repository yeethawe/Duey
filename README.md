# Duey
A minimal read-only implementation of the [NX PKG4.1 format](http://nxformat.github.io/) specification on .NET Standard 2.0

## 🤔 Why?
* Duey works on runtimes targetting or supporting .NET Standard!
* also, it's strictly parsing only. no caches, no weird voodoo magic.

## ✏️ Usage
to get started, simply create a new NX File object like so.
```csharp
var file = new NXFile("Data.nx");
```
with that, you can do various parsing magic!
```csharp
// store a node object for usage later on!
var node = file.Resolve("Products");

// resolve and defaults to null
var name = node.ResolveOrDefault<string>("name");

// resolve and defaults to a nullable
var stock = node.Resolve<int>("stock") ?? 0; // 0 is the default value!
var price = node.Resolve<double>("price") ?? 0.0;

// resolve a node ..in a node!
var bundles = node.Resolve("Bundled Products");

foreach (var bundle in bundles.Children)
{
    // resolve even more stuff here!
}

// if efficiency and speed is an issue..
foreach (var child in node) {
    var childName = child.Name;
    
    // this ensures that theres no extra lookup steps when parsing!
    switch (childName) {
        case "name": name = child.ResolveOrDefault<string>(); break;
        case "stock": stock = child.Resolve<int>(); break;
        case "price": price = child.Resolve<double>(); break;
    }
}
```
also, remember to dispose~!
```csharp
using (var file = new NXFile("Data.nx")) {
    // do your parsing thing here!
}

// or manually call dipose
var file = new NXFile("Data.nx");
file.Dispose();
```

## ⭐️ Acknowledgements
* [reNX](https://github.com/angelsl/ms-reNX) - main reference for implementations.
* [PKG1](https://labs.crr.io/maplestory/PKG1) - for the inspiration to create this project.
* all the kind souls who designed the NX format.
