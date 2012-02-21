!SLIDE smaller
# ActiveSupport::OrderedOptions #

    @@@ ruby
    require 'active_support'
    # true
    oo = ActiveSupport::OrderedOptions.new
    # #<OrderedHash {}>
    oo.role
    # nil
    oo.role = "admin"
    # "admin"
    oo.role
    # "admin"
    oo[:role]
    # "admin"

!SLIDE smaller
.notes OrderedOptions extends OrderedHash extends Hash.  Override the [] and []= to convert keys to symbols
# How It Works #
    @@@ ruby
    def respond_to?(name)
      true
    end

    def method_missing(name, *args)
      if name.to_s =~ %r(.*)=$/
        self[$1] = args.first
      else
        self[name]
      end
    end