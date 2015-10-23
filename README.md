# HABTM-tutorial
Cómo insertar, mediante un formulario, un elemento de un modelo que está relacionado con otro mediante "has and belongs to many" en Rails.
Si se tienen dos modelos:
```
class Product < ActiveRecord::Base
  has_and_belongs_to_many :branches
end

class Branch < ActiveRecord::Base
  has_and_belongs_to_many :products
end
```
