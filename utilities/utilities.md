!SLIDE smaller
# ActiveSupport::OrderedOptions #

    @@@ ruby

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
# ActiveSupport::StringInquirer #

    @@@ ruby

    irb> oo = ActiveSupport::OrderedOptions.new
    => #<OrderedHash {}>
    irb> oo.role = ActiveSupport::StringInquirer.new"admin"
    => "admin"
    irb> oo.role
    => "admin"
    irb> oo.role.admin?
    => true
    irb> oo.role == "admin"
    => true