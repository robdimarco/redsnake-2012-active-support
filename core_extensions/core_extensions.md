!SLIDE smaller
# Numeric Extensions #

    @@@ruby
    irb> require 'active_support/core_ext/numeric'
    => false
    irb> 5.days
    => 5 days
    irb> 5.days.ago
    => Mon Feb 06 16:45:28 -0500 2012
    irb> 1.year.from_now
    => Mon Feb 11 16:45:36 -0500 2013
    irb> 1.terabyte
    => 1099511627776
    irb> 1.month.from_now - 2.days
    => Fri Mar 09 16:46:11 -0500 2012

!SLIDE smaller
.notes We reopen the Ruby core class Numeric and add in methods
# How It Works #
    @@@ruby
    class Numeric
      def seconds
        ActiveSupport::Duration.new(self, 
          [[:seconds, self]])
      end
      alias :second :seconds

      # Reads best without arguments: 10.minutes.ago
      def ago(time = ::Time.current)
        time - self
      end
      #...
    end

!SLIDE smaller
# String Extensions #
    @@@ruby
    irb> require 'active_support/core_ext'
    => true
    irb> 'happy code'.titleize
    => "Happy Code"
    irb> "x-men: the last stand".titleize  # => "X Men: The Last Stand"
    => "X Men: The Last Stand"
    irb> 'python'.pluralize
    => "pythons"
    irb> 'ruby'.pluralize
    => "rubies"
    irb> "octopi".singularize
    => "octopus"
    irb> 'hello world'.parameterize
    => "hello-world"
    irb> 'hello world'.parameterize.underscore
    => "hello_world"
    irb> 'hello world'.parameterize.underscore.camelize
    => "HelloWorld"

!SLIDE smaller
# Object Extensions #
    @@@ruby
    irb> require 'active_support/core_ext'
    => true
    irb(main):030:0> class HappyCode; attr_accessor :val; end
    => nil
    irb(main):031:0> hc = HappyCode.new
    => #<HappyCode:0x1027ec2e8>
    irb(main):032:0> hc.val
    => nil
    irb(main):033:0> hc.val.try(:capitalize)
    => nil
    irb(main):034:0> hc.val = "burger"
    => "burger"
    irb(main):036:0> hc.val.try(:capitalize)
    => "Burger"

!SLIDE smaller
# How It Works #
.notes Open up both Object and Nil 
    @@@ruby
    class Object
      def try(*a, &b)
        if a.empty? && block_given?
          yield self
        else
          __send__(*a, &b)
        end
      end
    end

    class NilClass
      def try(*args)
        nil
      end
    end
    