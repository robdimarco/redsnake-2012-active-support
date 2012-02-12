!SLIDE smaller
# Numeric Extensions #

    @@@ruby
    require 'active_support/core_ext/numeric'
    # false
    
    5.days
    # 5 days
    
    5.days.ago
    # Mon Feb 06 16:45:28 -0500 2012
    
    1.year.from_now
    # Mon Feb 11 16:45:36 -0500 2013
    
    1.terabyte
    # 1099511627776
    
    1.month.from_now - 2.days
    # Fri Mar 09 16:46:11 -0500 2012

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
    'happy code'.titleize
    # "Happy Code"

    "x-men: the last stand".titleize
    # "X Men: The Last Stand"

    'python'.pluralize
    # "pythons"
    
    'ruby'.pluralize
    # "rubies"
    
    "octopi".singularize
    # "octopus"
    
    'hello world'.parameterize
    # "hello-world"
    
    'hello world'.parameterize.underscore.camelize
    # "HelloWorld"

!SLIDE smaller
# Object Extensions #
    @@@ruby
    class HappyCode; attr_accessor :val; end
    # nil
    
    hc = HappyCode.new

    hc.val.nil ? nil : hc.val.capitalize
    # Unhappy code
    
    hc.val.try(:capitalize)
    # nil
    
    hc.val = "burger"
    hc.val.try(:capitalize)
    # "Burger"

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
    