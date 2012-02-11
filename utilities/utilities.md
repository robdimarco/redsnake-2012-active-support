
!SLIDE smaller
# ActiveSupport::StringInquirer #
    @@@ ruby
    irb> require 'active_support'
    => true
    irb> role = ActiveSupport::StringInquirer.new "admin"
    => "admin"
    irb> role == "admin"
    => true
    irb> role.admin?
    => true

!SLIDE smaller
.notes StringInquirer extends String
# How It Works #
    @@@ ruby
    def method_missing(method_name, *arguments)
      if method_name.to_s[-1,1] == "?"
        self == method_name.to_s[0..-2]
      else
        super
      end
    end

!SLIDE smaller
# ActiveSupport::OrderedOptions #

    @@@ ruby
    irb> require 'active_support'
    => true
    irb> oo = ActiveSupport::OrderedOptions.new
    => #<OrderedHash {}>
    irb> oo.role
    => nil
    irb> oo.role = "admin"
    => "admin"
    irb> oo.role
    => "admin"
    irb> oo[:role]
    => "admin"

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