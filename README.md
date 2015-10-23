# HABTM-tutorial
Cómo insertar, mediante un formulario, un elemento de un modelo que está relacionado con otro mediante "has and belongs to many" en Rails.
Si se tienen dos modelos:
```ruby
class Product < ActiveRecord::Base
  has_and_belongs_to_many :branches
end

class Branch < ActiveRecord::Base
  has_and_belongs_to_many :products
end
```

Se hizo la siguiente migración, para generar la tabla intermedia de la relación:
```ruby
class CreateBranchesAndProducts < ActiveRecord::Migration
  def change
    create_table :branches_products, id: false do |t|
      t.belongs_to :branch, index: true
      t.belongs_to :product, index: true
    end
  end
end

```

Luego, 
