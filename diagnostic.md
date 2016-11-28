# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

```sh
Join tables are valuable because they allow developers to link the information in each table together via the join table, which allows us to compartemantalize
data in a more safe and efficient manner. It also prevents accidental data deletion because it changes the dyanimcs of the information being accessed and linked.
```

1.  Provide a database table structure and explain the Entity Relationship that
describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
(Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
`Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
join table with references to `Movies` and `Profiles`.

```sh
  Profiles>----<Favorites>---<Moveies

Many different profiles have many favorites and many favorites are many different movies.

```

1.  For the above example, what needs to be added to the Model files?

```rb
class Profile < ActiveRecord::Base
has_many :movie, through: :favorite
has_many :favorite, dependent: :destroy
end
```

```rb
class Movie < ActiveRecord::Base
has_many :profile, through: :favorite
has_many :favorite, dependent: :destroy
end
```

```rb
class Favorite < ActiveRecord::Base
belongs_to :proile, inverse_of: :favorite
belongs_to :movie, inverse_of: :favorite
end
```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

```sh
The serializer determines what data is displayed on the website, it can be used
to hide data that is present.

```

```rb
class ProfileSerializer < ActiveModel::Serializer
attributes :id, :given_name, :surname, :email\
has_one :profile
has_one :movie
end
```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

```sh
bundle exec rails g scaffold Favorites profile_id:integer movie_id:integer
```

1.  What is `Dependent: Destroy` and where/why would we use it?

```sh
"That is used in a table's model that is being connected to another table via a join table. We use it to insure that this data cannot be deleted; it's join table has to be deleted because this table AND it's partner join table are both dependents of the join table they're both connected to. If we didnt have this line, we could delete information from joined table A or joined table B, resulting in orphaned data. Those words prevent this. "
```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

```sh
A case can have a one-to-many relationship with judges and judges can have a many-to-many relationship with jury. 
```
