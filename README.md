# jrdspace

jrdspace enables scripting of  [DSpace](https://github.com/DSpace/DSpace) Java objects from the dspace-api package. 

It needs to run under jruby and uses environment variables to locate a local DSpace installation directory and determine other defaults. 

## Installation

### Prerequisite
 * JRuby  [Get Started](http://jruby.org/getting-started)
 * Package Manager  [Bundler](http://bundler.io/)
 * optional - but useful [RMV](https://rvm.io/)

### Installation 

Add the gem to your projects Gemfile: 
```
gem 'jrdspace', :git => 'https://github.com/akinom/dspace-jruby', :branch => 'master`
```

Install the gem:
```
bundle install
```

## Usage

Run one of the included executabes, see the [bin directory](/bin); for example

```
bundle exec netid_edit
bundle exec idspace
```

## Interactive Usage 

The included idspace command starts an interactive console.

```
bundle exec idspace 
> require 'dspace'
> DSpace.load          # load DSpace jars and configurations 
> DSpace.help          # lists all static methods of classes in jrdspace 
```

Use Tab completion to see available methods 

# Get Started 

Connect to your dspace database and configurations 

```
DSpace.load                                  # load DSpace jars and configurations from ENV[DSPACE_HOME] 
                                             # or from /dspcae if DSPACE_HOME is undefined
DSpace.load("/home/you/installs/dspace")     # load from /home/you/installs/dspace
```

After a succesfull load you can start using the included classes DSpace, DCommunity, DCollection, ... from [lib/dspace](lib/dspace) 

If you want to make changes you can 'login' 

```
DSpace.login               # login with ENV['USER']
DSpace.login ('netid')     # login with given netid
```

Remember to call the commit methods if you want changes to be saved to the database 

```
DSpace.commit 
```


Find Dspace stuff:

```
DSpace.fromHandle ('xxxxx/yyy')      
DSpace.fromString ('ITEM.124')      
DSpace.fromString ('GROUP.Anonymous')      
DSpace.fromString ('EPERSON.a_netid')   
DCommunity.all 
DGroup.find('group_name')
```
These methods use the relevant Java classes to locate the objects and return nil or a reference to the Java object found. All public Java methods can be called on returned refernces. 

The following prints a rudimentary community report:
```
DCommunity.all.each { |c| puts [c.getCollections.length, c.getHandle, c.getName].join("\t") }
```

Java objects can be converted to corresponding jrdspace objects, so that the additional functionality implemented by jrdspace classes becomes available. 
For example all jrdspace objects implement the parents and policies method:
```
dso = DSpace.fromHandle('xxxxx/zzz') 
DSpace.create(dso).parents
DSpace.create(dso).policies
```


# Thanks 
Thank you https://github.com/kardeiz/dscriptor to set me on the path to jruby with dspace 
