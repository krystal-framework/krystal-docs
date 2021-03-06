Cache component
===============

This component provides several services to manage caching functionality. Before we start using it, let's take a look at how to configure it first.

# Configuration

The configuration is located under `cache` key. It takes an array of arguments that defines cache engine type with its options. Typically it looks like so:

    cache => array(
       'engine' => '....',
       'options' => array(
         // Here come engine options 
      )
    )

Now let's take a look at available engines with their associated options, before we start using the cache service.

## File

This engine stores cache data in a file. Its configuration looks as the following:

    cache => array(
       'engine' => 'file',
       'options' => array(
         // This path is recommended, but you're free to defined another one.
         'file' => 'data/cache/cache.data',
      )
    )

Also, there's a hidden default options which is called `auto_create`. By default its value is `true`. The option itself defines whether to create a cache file if it doesn't exist or not.

## SQL

This engine stores cache data in SQL database. Its configuration looks as the following:

    cache => array(
       'engine' => 'sql',
       'options' => array(
          'connection' => '..connection name..',
          'table' => 'cache_table'
      )
    )

As you can see, it has only two options. The `connection` defines what connection to use (its name must be defined under `db` configuration) and the `table` option defines what table to use.

The table will not be created automatically for you. You have to do that yourself. So, the table schema is located at `/vendor/Krystal/Cache/Sql/table-schema.sql`. you have to execute that file in your SQL terminal (or any other tool of your choice that you use to manage a database).

# Memcached

If you have Memcached installed, you can use it as engine as well. The configuration might look like as following:

    'cache' => array(
       'engine' => 'memcached',
       'options' => array(
          'servers' => array(
             array('mem1.domain.com', 11211, 33),
             array('mem2.domain.com', 11211, 67)
          )
       )
    )


## Another engines

If you have installed something of this: WinCache, APC, XCache, you can use them as a cache engine. The following list represent their engine names.

`wincache` - WinCache
`apc` - APC
`xcache` - XCache

They have no options. Here's an example of defining configuration for WinCache:

    'cache' => array(
       'engine' => 'wincache'
    )

# Usage

Once configuration is done, you can start using a service. The service can be accessed in controllers, as a `cache` property, just like this:

    public function someAction()
    {
       // if the count key exists, return its value, otherwise return null
       $count = $this->cache->get('count', null);
    }

# Available methods

## increment()

    \Krystal\Cache\CacheEngineInterface::increment($key, $step = 1)

Increments a numeric key in a cache. By default the step is 1.

## decrement()

    \Krystal\Cache\CacheEngineInterface::decrement($key, $step = 1)

Decrements a numeric key in a cache. By default step is 1.

## set()

    \Krystal\Cache\CacheEngineInterface::set($key, $value, $ttl)

Stores a key in a cache. The third `$ttl` arguments defines a lifetime in seconds.

## get()

    \Krystal\Cache\CacheEngineInterface::get($key, $default = false)

Returns an entry from a cache. If one doesn't exist, then a value of the second argument is returned, which is false by default.

## remove()

    \Krystal\Cache\CacheEngineInterface::remove($key)

Removes an entry from a cache. If successes then returns `true`, otherwise `false`.

## has()

    \Krystal\Cache\CacheEngineInterface::has($key)

Determines whether cache key exist. Return boolean.

## getAll()

    \Krystal\Cache\CacheEngineInterface::getAll()

Returns all cache data.
