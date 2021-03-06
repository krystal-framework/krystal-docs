
Session component
================

This component provides a service object to work with session data.

# Configuration

In most, cases there's no need to configure the session service. But in case you want to change a default session handler and store all session data in a database or if you want to override default options for the cookies, you're free to do so. 

Now, let's take a look at what we can tweak.

## Database handler

First of all, open framework's configuration file and find a line which describes session configuration. It looks like this:

    session => array(
       'handler' => 'native'
    )

And replace it with the following:

    'session' => array(
      'handler' => 'sql',
      'options' => array(
       'connection' => 'mysql',
       'table' => 'sessions'
       )
    )

This tells the session's service component to use a database handler instead of native one.

Now let's de-construct that step by step.  The `handler` key defines a session handler. It can be either `native` or `sql`. The `options` key defines options for the handler. The SQL handler itself has two options : `connection` and `table`. The `connection` key defines the name of database connection to be used (it must be defined in `db` section), and the `table` key defines a table of the table that stores session data.

Note, before you start using the database handler, you have to create a table first. It's structure is located at `/vendor/Krystal/Session/Adapter/sql.schema.sql`. You have to execute that file first in SQL manager of your choice.


## Cookie parameters

To override default cookie parameters, you'd create a key name `cookie_params` right after `handler`. It would look as following:

    session => array(
       'handler' => 'native',
       'cookie_params' => array(
          // Options can be set here
       )
    )

Possible options are (taken from docs):

`lifetime` - Lifetime of the session cookie, defined in seconds. Must be an integer.

`path` - Path on the domain where the cookie will work. Use a single slash ('/') for all paths on the domain. 

`domain` - Cookie domain, for example 'www.php.net'. To make cookies visible on all subdomains then the domain must be prefixed with a dot like '.php.net'. 

`secure` - If `true` cookie will only be sent over secure connections.

`httponly` - If set to `true` then PHP will attempt to send the `httponly` flag when setting the session cookie. 


# Working with session

To work with session data, you can access its service called `sessionBag` in controllers, just like this:

    public function someAction()
    {
        $this->sessionBag->set('foo', 'bar');
    }

The session's service has a number of useful methods to work with a session. It's time to explore them.

## set()

\Krystal\Session\SessionBag::set($key, $value)

Stores a key and its value in the session.

## get()

    \Krystal\Session\SessionBag::get($key, $default = false)

Returns key's value from the session. In case a key doesn't exist, then default value is returned (which is `false` by default).

## has()

    \Krystal\Session\SessionBag::has($key)

Determines whether session key has been set. Returns boolean.

## remove()

    \Krystal\Session\SessionBag::remove($key)

Removes session value by its associated key, If a key doesn't exist, then `RuntimeException` will be thrown.

## removeAll()

    \Krystal\Session\SessionBag::removeAll()

Removes all data from the session.

## getAll()

    \Krystal\Session\SessionBag::getAll()

Returns all data from the session.

## isValid()

    \Krystal\Session\SessionBag::isValid()

Checks if the session is valid. This test is based on IP + Browser of the current user, so it can be used to prevent session hijacks.

## regenerate()

    \Krystal\Session\SessionBag::regenerate()

Regenerates session id.

## setName()

    \Krystal\Session\SessionBag::setName($name)

Defines new session name.

## getName()

    \Krystal\Session\SessionBag::getName()

Returns session name.

## setId()

    \Krystal\Session\SessionBag::setId($id)

Defines new session id.

## getId()

    \Krystal\Session\SessionBag::getId()

Returns unique session id.
