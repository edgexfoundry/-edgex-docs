# DTO Validation

The [go-mod-core-contracts](https://github.com/edgexfoundry/go-mod-core-contracts/) leverage the  [go-playground/validator](https://github.com/go-playground/validator/) for DTO validation as it provides common validation function and customization mechanism.

## Tag usage
EdgeX verifies the struct fields by using go-playground/validator validation tags or custom validation tags, for [example](https://github.com/edgexfoundry/go-mod-core-contracts/blob/main/dtos/device.go):
```
type Device struct {
	DBTimestamp    `json:",inline"`
	Id             string                        `json:"id,omitempty" validate:"omitempty,uuid"`
	Name           string                        `json:"name" validate:"required,edgex-dto-none-empty-string,edgex-dto-rfc3986-unreserved-chars"`
	Description    string                        `json:"description,omitempty"`
	AdminState     string                        `json:"adminState" validate:"oneof='LOCKED' 'UNLOCKED'"`
	OperatingState string                        `json:"operatingState" validate:"oneof='UP' 'DOWN' 'UNKNOWN'"`
	...
}
```
The device name field contains the following validation:

- **required** validation tag ensures the value is not zero, empty string, or nil
- **edgex-dto-none-empty-string** validation tag trims white space and ensures the value that is not an empty string
- **edgex-dto-rfc3986-unreserved-chars** validation tag checks the value that does not contain reserved characters

You can find more validations in the [go-playground/validator](https://pkg.go.dev/github.com/go-playground/validator/v10) and EdgeX custom validations in the [go-mod-core-contracts](https://github.com/edgexfoundry/go-mod-core-contracts/blob/main/common/validator.go).

## Character restriction
The EdgeX uses the custom validation **edgex-dto-rfc3986-unreserved-chars** to prevent the user inputting the reserved characters.

This validation allows for only the following characters:

- alphabet: a-z, A-Z
- digital number: 0-9
- special character: - _ ~ : ; =

!!! edgey "EdgeX 2.3"
    We need to reduce the character restriction for the following use cases:

     - In BACNet protocol, the user might combine object type and property as the resourceName, for example, analog_input_0:present-value
     - In OPC_UA protocol, the user might use NodeId as the resourceName, for example, ns=10;s=Hello:World
