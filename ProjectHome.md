# Introduction #

The purpose is to invalidate a collection of keys at once - inspired by the memcached-tag project. I was looking for something simpler, and more compatible so I wrote my own.

# Features #

  * Each key can have multiple flags
  * Each flag and key holds a version number, flag has newer version than key - cache miss
  * Doesn't delete all keys related to flag, only invalidates
  * Only tries to initiate memcache connection once - cache miss returned at once on later requests


# Example usage #
```

require_once 'cache.class.php';
$cache = new cache(array('127.0.0.1'));

$key = 'test';
$value = rand(3,123);
$flags = array('page1','pic1');
$seconds_in_cache = 5;

echo "Value to store: $value <br />";
if ($cache->set($key,$value,$seconds_in_cache,$flags)) echo "Cache set successfully!<br />";

echo "Value stored in memcache: ".$cache->get('test');

$flag = 'pic1';
echo "Version of flag $flag bumped to ".$cache->invalidate($flag);

echo "Value from memcached for key after invalidation: {$cache->get($key)}<br />";

// For completeness
$cache->delete('test_flag');
$cache->increment('test_key');
```