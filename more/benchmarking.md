!SLIDE smaller
# Benchmarkable #
    @@@ruby
    require 'active_support'
    require 'logger'
    def logger
      @logger||= Logger.new(STDOUT)
      @logger
    end
    class HappyCode
      def expensive_operation_here
        sleep 5
        "foo"
      end
    end

    include ActiveSupport::Benchmarkable

    benchmark( "Expensive Operation"){ 
      HappyCode.new.expensive_operation_here
    }

    => INFO -- : Expensive Operation (5000.3ms)
    
!SLIDE smaller
# How It Works
    @@@ruby
    def benchmark(message = "Benchmarking", options = {})
      if logger
        options.assert_valid_keys(:level, :silence)
        options[:level] ||= :info

        result = nil
        ms = Benchmark.ms { 
          result = if options[:silence]
            logger.silence { yield }
          else
            yield 
          end
        }
        logger.send(options[:level], 
          '%s (%.1fms)' % [ message, ms ])
        result
      else
        yield
      end
    end