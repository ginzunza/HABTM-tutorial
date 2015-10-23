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

Luego, para el caso de la inserción de un nuevo producto, en donde se busque relacionar éste con una sucursal (branch) ya existente, el formulario sería de la siguiente manera:
```ruby
<%= form_for(@product) do |f| %>
  <%= f.label :name, "Nombre" %><br>
  <%= f.text_field :name %>
  <%= f.fields_for :branches do |u|%>
    <%= u.label :id, "Sucursal" %><br>
    <%= u.select(:id, Branch.all.collect {|p| [p.name, p.id]}, {include_blank: "Ninguna"}) %>
  <% end %>
<% end %>
```
Por simplicidad, se asumió que el producto posee sólo el atributo "name", lo mismo para sucursales. Como :branches es un atributo embebido de producto, para que se pueda utilizar en el formulario, se debe modificar el modelo de Products, el cual queda de la siguiente manera:
```ruby
class Product < ActiveRecord::Base
  has_and_belongs_to_many :branches
  accepts_nested_attributes_for :branches
end
```
También, para que se visualice el select de branches, dado a que no hay ninguna relación, la acción "new" de product se debe modificar, quedando de la siguiente manera:
```ruby
def new
  @product = Product.new
  @product.branches.build
end
```
