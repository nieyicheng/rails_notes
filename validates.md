#### validates

```ruby
validates :wage_lower_bound, numericality: { greater_than: 0} #数字验证，大于0

validates :status, length: { minimum: 3 } #长度验证
                      length: { minimum: 0, maximum: 2000} #范围验证

validates :status, presence: true  #存在性验证

#邮箱验证
VALID_EMAIL_REGEX = /\A[\w+\-.]+@[a-z\d\-]+(\.[a-z\d\-]+)*\.[a-z]+\z/i 
validates :email, presence: true, length: { maximum: 255 },
                  format:     { with: VALID_EMAIL_REGEX }
				  uniqueness: { case_sensitive: false }
				 #唯一性验证





```

