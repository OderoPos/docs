# ECR integration

## App Setup
Login in ECR app with `integrator` user type.
A list with all the integrated apps on the ECR330 will be displayed.

If we want to mark an app as integrated with the ECR330 app we need to define in the manifest file of the target app the following metadata.

> AndroidManifest.xml

```xml
<meta-data
    android:name="app_name"
    android:value="ECR_[app name]" />
```
After that, the app will be displayed on the main screen of the ECR330 app.


## Fiscal commands

libVersion = Check latest version [here](https://ro-artifactory.devtokeninc.com/ui/native/PublicLibraries/X330/integrationLibs/fiscal)

**_NOTE:_**  For best compatibility `libVersion` should match ECR version installed

> Dependencies

```groovy
//in settings.gradle
maven {
    url 'https://ro-artifactory.devtokeninc.com/artifactory/PublicLibraries/'
}

//in app.gradle
implementation 'X330.integrationLibs:fiscal:$libVersion
```


Use `EcrCommandExecutor` object for sending commands to the ECR and receiving the result.

The fiscal commands are sent to the ECR using the `executeFiscalCommand` method. The result can be received
as a `Flow` or if you want to receive as a `EcrCommandResultListener`.

The user screen commands follow the same `flow`, `listener` pattern.

Type of fiscal commands

`SetDateTime`  
`SetCompanyData`  
`GetCompanyData`  
`SetVATRates`  
`GetVATRates`  
`Fiscalise`  
`InitElectronicJournal`  
`TechnicalReport`  
`ZReport`  
`XReport`  
`InitJournal`  
`Journal`  
`JournalExport`  
`XJournal`  
`CustomerData`  
`PeriodicReport`  
    - `ByDate`  
    - `ByZ`  
`XMLReport`  
`GetDailyTotals`  
`GetStatus`  
`GetSoftwareVersion`  
`LoadProfile`  
`ResetFiscalMemory`  
`Sale`  
`CashOperation`  

> Fiscal commands

```kotlin
//Example of usage for sending a fiscal command and receiving the result as a [Flow]:
EcrCommandExecutor.executeFiscalCommand(
   context = context,
   command = FiscalCommand.SetDateTime(Calendar.getInstance().time),
).collect { result ->
    result.runCatching {
        Log.d("MainActivity", "Result.Success: $result")
        Toast.makeText(context, result.toString(), Toast.LENGTH_SHORT)
            .show()
    }.onFailure {
        Log.e("MainActivity", "Result.Failure: $result")
        Toast.makeText(context, result.toString(), Toast.LENGTH_SHORT)
            .show()
     }
}

//Example of usage for sending a fiscal command and receiving the result as a [EcrCommandResultListener]:
EcrCommandExecutor.executeFiscalCommand(
   context = context,
   command = FiscalCommand.SetDateTime(Calendar.getInstance().time),
   listener = object : EcrCommandResultListener {
         override fun onEcrCommandResult(result: String) {
             // Handle the successful result
         }
         override fun onEcrCommandError(error: String) {
             // Handle the error result
         }
   },
)

//Example of usage for sending a user screen command and receiving the result as a [Flow]:
EcrCommandExecutor.executeUserScreenCommand(
   context = context,
   newText = SecureRandom().nextInt(100).toString(),
).collect { result ->
    result.runCatching {
        Log.d("MainActivity", "Result.Success: $result")
        Toast.makeText(context, result.toString(), Toast.LENGTH_SHORT)
            .show()
    }.onFailure {
        Log.e("MainActivity", "Result.Failure: $result")
        Toast.makeText(context, result.toString(), Toast.LENGTH_SHORT)
            .show()
    }
}

//Example of usage for sending a user screen command and receiving the result as a [EcrCommandResultListener]:
EcrCommandExecutor.executeUserScreenCommand(
   context = context,
   newText = SecureRandom().nextInt(100).toString(),
   listener = object : EcrCommandResultListener {
       override fun onEcrCommandResult(result: String) {
           // Handle the successful result
       }
       override fun onEcrCommandError(error: String) {
           // Handle the error result
       }
   },
)

//Example of a sale fiscal command
FiscalCommand.Sale(
    sellerInfo = SellerInfo(
        cashierName = "John Doe",
        posNumber = "123413",
    ),
    products = listOf(
        FiscalProduct(
            productName = "Product 1",
            unit = "kg",
            quantity = "1",
            pricePerUnit = "10,5",
            vatId = "B",
        ),
        FiscalProduct(
            productName = "Product 2",
            unit = "kg",
            quantity = "1",
            pricePerUnit = "15,5",
            vatId = "B",
        ),
    ),
    payments = listOf(
        FiscalPayment(
            paymentMethod = FiscalPayment.PaymentMethod.Card,
            amount = "26",
        ),
    ),
    cifNumber = "43511483",
)
```

## Sending a card payment

Please check out [PaymentGateway](https://developer.pos.odero.ro/#paymentgateway) docs