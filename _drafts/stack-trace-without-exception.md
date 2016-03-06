If you would like to see the stack trace thus far anywhere in your coding, you can use [Kernel.caller](http://apidock.com/ruby/Kernel/caller).

``` ruby
caller.each { |trace| puts trace }
```
