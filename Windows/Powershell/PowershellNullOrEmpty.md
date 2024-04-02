Checking for null values in PowerShell is a common scenario, as Null values can cause issues when used in arithmetic operations or string concatenation. It is essential to handle null values correctly to avoid errors. The -eq operator is used to check whether a value is null or not. The following code snippet shows how to check for null values in PowerShell:  


if ($variable -eq $null) {  
    Write-Host "Variable is null"  
}  
else {  
    Write-Host "The variable is not null."  
}  



$var = "Hello, world!"  
if ($var -ne $null) {  
    Write-Host "The variable is not null."  
} else {  
    Write-Host "The variable is null."  
}  



PowerShell IsNullOrEmpty Method  
PowerShell has a method for checking null value or empty: IsNullOrEmpty. The Is Null or Empty method in the [string] class is used to check whether a value is null or empty. The following code snippet shows how to use the Is Null or Empty statement in PowerShell:  


$variable = ""  
if ([string]::IsNullOrEmpty($variable)) {  
    Write-Host "Variable is null or empty"  
}  


PowerShell Is Not Null or Empty  
The Is Not Null or Empty statement is used to check whether a value is not null or empty. The following code snippet shows how to use the If Not Null or Empty statement in PowerShell:  


$variable = "some value"  
if (![string]::IsNullOrEmpty($variable)) {  
    Write-Host "Variable is not null or empty"  
}  
You can also use the -or operator to combine null and empty string checks in PowerShell. For example:  


if ($stringVariable -eq $null -or $stringVariable -eq "") {  
    Write-Host "The string variable is null or empty"  
}  

#Read more: https://www.sharepointdiary.com/2021/12/check-null-not-null-empty-in-powershell.html#ixzz8WDk9Dfp8
