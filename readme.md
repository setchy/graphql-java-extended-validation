# Extended Validation for graphql-java

This library provides extended validation of fields and field arguments for [graphql-java](https://github.com/graphql-java/graphql-java)


# Status

This code is current under construction.  There is no release and not all parts of it are ready.

But the project welcomes all feedback and input on code design and validation requirements. 

# Using the code

TODO

# SDL @Directive constraints

This library a series of directives that can be applied to field arguments and input type fields which will constrain their allowable values.

These names and semantics are inspired from the javax.validation annotations

https://javaee.github.io/javaee-spec/javadocs/javax/validation/constraints/package-summary.html

You can add these onto arguments or input types in your graphql SDL.

For example

```graphql

    #
    # this declares the directive as being possible on arguments and input fields
    #
    directive @Size(min : Int = 0, max : Int = 2147483647, message : String = "graphql.validation.Size.message")
                        on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION

    input Application {
        name : String @Size(min : 3, max : 100)
    }

    type Query {
        hired (applications : [Application!] @Size(max : 10)) : [Boolean]
    }
```

In the example above, we have a `applications` argument that takes at most 10 applications and within each `Application` input object,
the `name` field must be at least 3 characters long and no more than 100 characters long to be considered valid. 


## The supplied constraints

### @AssertFalse

The boolean value must be false.

- Example : `updateDriver( isDrunk : Boolean @AssertFalse) : DriverDetails`

- Applies to : `Boolean`

- SDL : `directive @AssertFalse(message : String = "graphql.validation.AssertFalse.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.AssertFalse.message`


### @AssertTrue

The boolean value must be true.

- Example : `driveCar( hasLicence : Boolean @AssertTrue) : DriverDetails`

- Applies to : `Boolean`

- SDL : `directive @AssertTrue(message : String = "graphql.validation.AssertTrue.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.AssertTrue.message`


### @DecimalMax

The element must be a number whose value must be less than or equal to the specified maximum.

- Example : `driveCar( bloodAlcoholLevel : Float @DecimalMax(value : "0.05") : DriverDetails`

- Applies to : `String`, `Byte`, `Short`, `Int`, `Long`, `BigDecimal`, `BigInteger`, `Float`

- SDL : `directive @DecimalMax(value : String!, inclusive : Boolean! = true, message : String = "graphql.validation.DecimalMax.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.DecimalMax.message`


### @DecimalMin

The element must be a number whose value must be greater than or equal to the specified minimum.

- Example : `driveCar( carHorsePower : Float @DecimalMin(value : "300.50") : DriverDetails`

- Applies to : `String`, `Byte`, `Short`, `Int`, `Long`, `BigDecimal`, `BigInteger`, `Float`

- SDL : `directive @DecimalMin(value : String!, inclusive : Boolean! = true, message : String = "graphql.validation.DecimalMin.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.DecimalMin.message`


### @Digits

The element must be a number inside the specified `integer` and `fraction` range.

- Example : `buyCar( carCost : Float @Digits(integer : 5, fraction : 2) : DriverDetails`

- Applies to : `String`, `Byte`, `Short`, `Int`, `Long`, `BigDecimal`, `BigInteger`, `Float`

- SDL : `directive @Digits(integer : Int!, fraction : Int!, message : String = "graphql.validation.Digits.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.Digits.message`


### @Max

The element must be a number whose value must be less than or equal to the specified maximum.

- Example : `driveCar( horsePower : Float @Max(value : 1000) : DriverDetails`

- Applies to : `Byte`, `Short`, `Int`, `Long`, `BigDecimal`, `BigInteger`, `Float`

- SDL : `directive @Max(value : Int! = 2147483647, message : String = "graphql.validation.Max.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.Max.message`


### @Min

The element must be a number whose value must be greater than or equal to the specified minimum.

- Example : `driveCar( age : Int @Min(value : 18) : DriverDetails`

- Applies to : `Byte`, `Short`, `Int`, `Long`, `BigDecimal`, `BigInteger`, `Float`

- SDL : `directive @Min(value : Int! = 0, message : String = "graphql.validation.Min.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.Min.message`


### @Negative

The element must be a negative number.

- Example : `driveCar( lostLicencePoints : Int @Negative) : DriverDetails`

- Applies to : `Byte`, `Short`, `Int`, `Long`, `BigDecimal`, `BigInteger`, `Float`

- SDL : `directive @Negative(message : String = "graphql.validation.Negative.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.Negative.message`


### @NegativeOrZero

The element must be a negative number or zero.

- Example : `driveCar( lostLicencePoints : Int @NegativeOrZero) : DriverDetails`

- Applies to : `Byte`, `Short`, `Int`, `Long`, `BigDecimal`, `BigInteger`, `Float`

- SDL : `directive @NegativeOrZero(message : String = "graphql.validation.NegativeOrZero.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.NegativeOrZero.message`


### @NotBlank

The String must contain at least one non-whitespace character, according to Java's Character.isWhitespace().

- Example : `updateAccident( accidentNotes : String @NotBlank) : DriverDetails`

- Applies to : `String`

- SDL : `directive @NotBlank(message : String = "graphql.validation.NotBlank.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.NotBlank.message`


### @NotEmpty

The element must have a non zero size.

- Example : `updateAccident( accidentNotes : [Notes]! @NotEmpty) : DriverDetails`

- Applies to : `String`, `Lists`, `Input Objects`

- SDL : `directive @NotEmpty(message : String = "graphql.validation.NotEmpty.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.NotEmpty.message`


### @Pattern

The String must match the specified regular expression, which follows the Java regular expression conventions.

- Example : `updateDriver( licencePlate : String @Patttern(regex : "[A-Z][A-Z][A-Z]-[0-9][0-9][0-9]") : DriverDetails`

- Applies to : `String`

- SDL : `directive @Pattern(regexp : String! =".*", message : String = "graphql.validation.Pattern.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.Pattern.message`


### @Positive

The element must be a positive number.

- Example : `driver( licencePoints : Int @Positive) : DriverDetails`

- Applies to : `Byte`, `Short`, `Int`, `Long`, `BigDecimal`, `BigInteger`, `Float`

- SDL : `directive @Positive(message : String = "graphql.validation.Positive.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.Positive.message`


### @PositiveOrZero

The element must be a positive number or zero.

- Example : `driver( licencePoints : Int @PositiveOrZero) : DriverDetails`

- Applies to : `Byte`, `Short`, `Int`, `Long`, `BigDecimal`, `BigInteger`, `Float`

- SDL : `directive @PositiveOrZero(message : String = "graphql.validation.PositiveOrZero.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.PositiveOrZero.message`


### @Range

The element range must be between the specified `min` and `max` boundaries (inclusive).  It accepts numbers and strings that represent numerical values.

- Example : `driver( milesTravelled : Int @Range( min : 1000, max : 100000)) : DriverDetails`

- Applies to : `String`, `Byte`, `Short`, `Int`, `Long`, `BigDecimal`, `BigInteger`, `Float`

- SDL : `directive @Range(min : Int = 0, max : Int = 2147483647, message : String = "graphql.validation.Range.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.Range.message`


### @Size

The element size must be between the specified `min` and `max` boundaries (inclusive).

- Example : `updateDrivingNotes( drivingNote : String @Size( min : 1000, max : 100000)) : DriverDetails`

- Applies to : `String`, `Lists`, `Input Objects`

- SDL : `directive @Size(min : Int = 0, max : Int = 2147483647, message : String = "graphql.validation.Size.message") on ARGUMENT_DEFINITION | INPUT_FIELD_DEFINITION`

- Message : `graphql.validation.Size.message`
