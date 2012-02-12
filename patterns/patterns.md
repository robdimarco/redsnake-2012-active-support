!SLIDE smaller
# Common Pattern
    @@@ruby
    def long_running_operation
      @lro ||= do_work_here
      @lro
    end

!SLIDE smaller
# Using Memoizable #
    @@@ruby
    extend ActiveSupport::Memoizable
    def long_running_operation
      do_work_here
    end
    memoize :expensive_operation_here
    
    # later on...clear with 
    flush_cash :expensive_operation_here
    
!SLIDE smaller
# How It Works
    @@@ruby
    # In a nutshell...
    def memoize(*symbols)
      symbols.each do |symbol|
        original_method = :"_unmemoized_#{symbol}"
        memoized_ivar = 
          ActiveSupport::Memoizable.memoized_ivar_for(symbol)
          
        alias #{original_method} #{symbol}
        
        class_eval "def #{symbol}(reload = false)
          if reload || !defined?(#{memoized_ivar}) || 
            #{memoized_ivar}.empty?
            
            #{memoized_ivar} = [#{original_method}]n  
            
          end
                                               
          #{memoized_ivar}[0]
          
        end"
      end
    end