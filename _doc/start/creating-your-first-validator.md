---
title: Creating your first validator
---

To define a set of validation rules for a particular object, you will need to create a class that inherits from `ValidatorBase<T>`, where `T` is the type of class that you wish to validate. 

For example, imagine that you have a Customer class:

```csharp
public class Customer {
  public int Id { get; set; }
  public string Surname { get; set; }
  public string Forename { get; set; }
  public decimal Discount { get; set; }
  public string Address { get; set; }
}
```

You would define a set of validation rules for this class by inheriting from `ValidatorBase<Customer>`:

```csharp
using FluentValidation; 

public class CustomerValidator : ValidatorBase<Customer> {
}
```

The validation rules themselves should be defined in the validator class's `Rules` method, which must be overridden.

To specify a validation rule for a particular property, call the `RuleFor` method, passing a lambda expression 
that indicates the property that you wish to validate. For example, to ensure that the `Surname` property is not null, 
the validator class would look like this:

```csharp
using FluentValidation;

public class CustomerValidator : ValidatorBase<Customer> {
  protected override void Rules() {
    RuleFor(customer => customer.Surname).NotNull();
  }
}
```
To run the validator, instantiate the validator object and call the `Validate` method, passing in the object to validate.

```csharp
Customer customer = new Customer();
CustomerValidator validator = new CustomerValidator();

ValidationResult result = validator.Validate(customer);

```

The `Validate` method returns a ValidationResult object. This contains two properties:

- `IsValid` - a boolean that says whether the validation suceeded.
- `Errors` - a collection of ValidationFailure objects containing details about any validation failures.

The following code would write any validation failures to the console:

```csharp
Customer customer = new Customer();
CustomerValidator validator = new CustomerValidator();

ValidationResult results = validator.Validate(customer);

if(! results.IsValid) {
  foreach(var failure in results.Errors) {
    Console.WriteLine("Property " + failure.PropertyName + " failed validation. Error was: " + failure.ErrorMessage);
  }
}
```
