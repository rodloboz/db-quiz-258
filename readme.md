# Quiz 3 - DB

## Q1

Relationship DBs store data in tables, made up of rows using primary_keys

## Q2

 1-1: One to One it doesnt matter where the FK is stored
 1-N: One to Many FK is stored in the Many / Children)
 N-N: Many to Many needs a JOIN table

## Q3

![DB Schema Screenshot](schema.png?raw=true)

## Q4

![DB Schema Screenshot](schema2.png?raw=true)


## Q5

SQL - Structured Query Language


# Q6

```sql
SELECT * FROM books
WHERE published_year < 1985
```

#Q7

```sql
SELECT * FROM books
JOIN authors ON books.author_id = authors.id
WHERE authors.name = 'Jules Verne'
ORDER BY books.published_year DESC
LIMIT 3
```

#Q8

ActiveRecord abtracts away SQL queries
Allows us to user ruby to interact with the DB

#Q9

Migrations apply/change the schema/structure of the DB

```bash
rake db:migrate
```

#Q10

*Authors*
```ruby
class CreateAuthors < ActiveRecord::Migration[5.1]
  def change
    create_table :authors do |t|
      t.string :name

      t.timestamps
    end
  end
end
```

*Books*
```ruby
class CreateBooks < ActiveRecord::Migration[5.1]
  def change
    create_table :books do |t|
      t.string :title
      t.integer :published_year
      t.references :author, foreign_key: true

      t.timestamps
    end
  end
end
```

*Users*
```ruby
class CreateUsers < ActiveRecord::Migration[5.1]
  def change
    create_table :users do |t|
      t.string :email

      t.timestamps
    end
  end
end
```

*Readings*
```ruby
class CreateReadings < ActiveRecord::Migration[5.1]
  def change
    create_table :readings do |t|
      t.references :users, foreign_key: true
      t.references :book, foreign_key: true
      t.datetime :read_at

      t.timestamps
    end
  end
end
```

## Q11

*Change Books*
```ruby
class AddCategoryToBooks < ActiveRecord::Migration[5.1]
  def change
    add_column :books, :category, :string
  end
end
```

## Q12

*Author Model*
```ruby
# author.rb
class Author < ActiveRecord::Base
  has_many :books
end
```

*Book Model*
```ruby
# book.rb
class Book < ActiveRecord::Base
  belongs_to :author
  has_many :readings
end
```

*User Model*
```ruby
# user.rb
class User < ActiveRecord::Base
  has_many :readings
  has_many :books, through: :readings
end
```

*Reading Model*
```ruby
# reading.rb
class Reading < ActiveRecord::Base
  belongs_to :user
  belongs_to :book
end
```

## Q13

```ruby
#1. Add your favorite author to the DB
author = Author.create(name: 'Jack Kerouac')

#2. Get all authors
authors = Author.all

#3. Get author with id=8
author = Author.find(8)

#4. Get author with name="Jules Verne", store it in a variable: jules
author = Author.find_by(name: "Jules Verne")
# author = Author.where(name: "Jules Verne").first
# author = Author.find_by_name("Jules Verne")

#5. Get Jules Verne's books
books = author.books

#6. Create a new book "20000 Leagues under the Sea", it's missing
# in the DB. Store it in a variable: twenty_thousand
# twenty_thousand = Book.new({
#   title: "20000 Leagues under the Sea",
#   published_year: 1870
# })
# twenty_thousand.author = author # "Jules Verne"
# twenty_thousand.save

#7. Add Jules Verne as this book's author
twenty_thousand.author = author # "Jules Verne"

#8. Now save this book in the DB!
twenty_thousand.save


# The last 3 steps can be done in 1
# author is the "Jules Verne" instance
author.books.create(title: "20000 Leagues under the Sea", published_year: 1870)
# The end
```

## Q14

*Author Model*
```ruby
# author.rb
class Author < ActiveRecord::Base
  has_many :books

  validates :name, presence: true, uniqueness: true
end
```

















