
!SLIDE smaller
# ActiveSupport::StringInquirer #
    @@@ ruby
    require 'active_support'
    # true
    role = ActiveSupport::StringInquirer.new "admin"
    # "admin"
    role == "admin"
    # true
    role.admin?
    #  true

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