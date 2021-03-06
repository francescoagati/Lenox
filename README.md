Lenox
=====

Lean compile-time Lense macro generation Library.
Lenses purpose is to ease working using immutable data, removing boilerplate.

## Definition

```haxe
typedef User = {
	name : String,
	age : Int
}
class User_ implements LensesFor<User> {}

typedef Group = {
	leader : User,
	followers : Array<User>
}
class Group_ implements LensesFor<Group> {}

```

## Usage

```haxe
		var user : User =  {
			name : "Stephane",
			age : 40
		};
		
		// These abstractions are now accessible:
		var userName : Lense<User, String> = User_.name_; 
		var userAge : Lense<User, Int> = User_.age_;

		var newUser = user.mod(User_.name_, function (prevName) return prevName.toUpperCase()); // new user has upprcased name
		var sldUser = user.set(User_.name_, "sld"); // new user named sld
		user.get(User_.age_); // 40
		
		// Thanks to the Lense abstraction we can then abstract over the manipulated object
		function withUppercaseMember<T>(t : T, l : Lense<T, String>) : T {
			return t.mod(l, function (v) return v.toUpperCase());
		}
		var updatedUser = withUppercaseMember(user, User_.name_);
		
		// and then compose Lenses; Profit!
		var group : Group = {
			leader : user,
			followers : []
		};
		var updatedGroup = withUppercaseMember(group, Group_.leader_.andThen(User_.name_));

		
```



