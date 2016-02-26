# crystal-db

Common db api for crystal

There is already a forked implementation for [sqlite](https://github.com/bcardiff/crystal-sqlite3/tree/db) and [mysql](https://github.com/bcardiff/crystal-mysql/tree/db) that (partially) implements mysql's binary protocol (no dependency!)

## Installation

Add this to your application's `shard.yml`:

```yaml
dependencies:
  db:
    github: bcardiff/crystal-db
```

## Usage

Since this is an abstract db api, it's usage is through a concrete database driver.

```crystal
require "db"
require "sqlite3"

DB.open "sqlite3:./file.db" do |db|
  db.exec "create table contacts (name string, age integer)"
  db.exec "insert into contacts values (?, ?)", "John Doe", 30

  args = [] of DB::Any
  args << "Sarah"
  args << 33
  db.exec "insert into contacts values (?, ?)", args

  puts "max age:"
  puts db.scalar "select max(age) from contacts" # => 33

  puts "contacts:"
  db.query "select name, age from contacts order by age desc" do |rs|
    puts "#{rs.column_name(0)} (#{rs.column_name(1)})"
    # => name (age)
    rs.each do
      puts "#{rs.read(String)} (#{rs.read(Int32)})"
      # => Sarah (33)
      # => John Doe (30)
    end
  end
end
```
## Roadmap

Issues not yet addressed

[ ] Support non prepared statements.
[ ] Time data type.
[ ] Data type extensibility. Allow each driver to extend the data types allowed.
[ ] Transactions.
[ ] Connection pool.
[ ] Direct access to `IO` to avoid memory allocation for blobs.

## Contributing

1. Fork it ( https://github.com/bcardiff/crystal-db/fork )
2. Create your feature branch (git checkout -b my-new-feature)
3. Commit your changes (git commit -am 'Add some feature')
4. Push to the branch (git push origin my-new-feature)
5. Create a new Pull Request

## Contributors

- [bcardiff](https://github.com/bcardiff) Brian J. Cardiff - creator, maintainer
