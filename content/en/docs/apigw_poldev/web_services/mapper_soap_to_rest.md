{
"title": "Expose a web service as a REST API",
"linkTitle": "Expose a web service as a REST API",
"date": "2019-10-17",
"weight": 5,
"description": "Import a WSDL file and define a REST API in Policy Studio to expose a web service as a REST API."
}

You can import a WSDL file into Policy Studio, and invoke it from a policy instead of exposing it to a client.

For example, in a SOAP to REST use case, the web service is registered in Policy Studio by importing a WSDL file into the web service repository. A REST API is then defined in Policy Studio, which calls a policy to implement the API, and in turn, this policy invokes the web service.

## Virtualize a SOAP web service

To expose a virtualized version of a SOAP web service on API Gateway, import a WSDL file describing the web service into the web service repository.

The following figure shows importing the WSDL for a currency conversion SOAP web service.

{{< alert title="Note" color="primary" >}}This is an example for demonstration purposes only. Adapt the following steps to suit your web service.{{< /alert >}}

![Import WSDL for SOAP web service](/Images/PolDevGuide/Mapper/import_wsdl.png)

For more information on using the **Import WSDL** wizard, see [Import a WSDL file](/docs/apigw_poldev/web_services/general_policy_wsdl/#import-a-wsdl-file).

When you register a web service in Policy Studio, service handlers and policies are autogenerated.

The following figure shows the generated policies for the currency conversion service.

![Generated policies for web service](/Images/PolDevGuide/Mapper/wsdl_gen_policies.png)

## Define a REST API

The next step is to define a REST API for a currency conversion service as follows:

1. Create a new policy container called `CurrencyConversion`.
2. Within this policy container create a new policy called `MainRouter`.
3. Add a **Policy Shortcut** filter to the `MainRouter` policy, and set it to call the `CurrencyConvertor` policy that was autogenerated when you imported the WSDL for the web service.
4. Add a **Validate REST** filter to the `MainRouter` policy. Set the **Method** to `GET`, add two request parameters called `fromCurrency` and `toCurrency`, and select the **Extract valid parameters into individual message attributes** check box:
    ![Validate REST filter](/Images/PolDevGuide/Mapper/validate_rest.png)
5. Add a relative path for the `MainRouter` policy so that all REST requests received by API Gateway on the path `/convertcurrency/getConversionRate` are processed by the `MainRouter` policy. On the **HTTP Method** tab of the path resolver, set the method to `GET`.

    ![Add relative path](/Images/PolDevGuide/Mapper/relative_path.png)

    At this stage, the MainRouter policy should look like this:

    ![MainRouter policy (1)](/Images/PolDevGuide/Mapper/mainrouter_policy_1.png)

## Route REST requests through the virtualized SOAP service

To route REST requests through the virtualized SOAP service, perform the following sequence of tasks.

### Create a request processing policy

First, create a dedicated request processing policy to create the SOAP request message body to send to the SOAP service:

1. Create a request processing policy called `GetCurrencyConvertorRequest`.
2. Add a **Set HTTP verb**
    filter to the policy and enter `POST`
    in the **HTTP Verb**
    field.
3. Add an **Add HTTP header**
    filter to the policy with the following settings:
    ![Add HTTP header](/Images/PolDevGuide/Mapper/add_http_header.png)
4. Add a **Set Message**
    filter to the policy and enter `text/xml`
    as the **Content-Type**.

    Select **From web service operation**
    from the **Populate**
    menu and select the `ConversionRate`
    operation from the currency conversion web service. This populates the contents of the message body.
    ![Set message](/Images/PolDevGuide/Mapper/set_message.png)

    You need to replace the currencies in the message body with the currencies from the incoming REST request. To enable the autocompletion mechanism in the **Set Message** filter, which will allow you to insert the `fromCurrency` and `toCurrency` selector strings in the message body, you must first connect up the filters in the `MainRouter` and `GetCurrencyConvertorRequest` policies.
5. Add a **Policy Shortcut** filter to the `MainRouter` policy, and set it to call the `GetCurrencyConvertorRequest` policy.
6. Connect the filters in the `MainRouter` policy as follows:

    ![MainRouter policy (2)](/Images/PolDevGuide/Mapper/mainrouter_policy_2.png)

7. Connect the filters in the `GetCurrencyConvertorRequest` policy as follows:

    ![GetCurrencyConvertorRequest policy](/Images/PolDevGuide/Mapper/getcurrencyconvertorrequest_policy.png)

8. Edit the **Set Message** filter. Select the currency type in the message body and start typing to see matching selectors.

    ![Set message autocompletion](/Images/PolDevGuide/Mapper/set_message_autocomplete.png)

9. Insert the selectors `$params.query.fromCurrency` and `$params.query.toCurrency` in place of the currency types.

    ![Set message with selectors](/Images/PolDevGuide/Mapper/set_message_selectors.png)

### Create a response processing policy

Next, create a response processing policy to convert the XML returned from the SOAP web service to JSON format:

1. Create a response processing policy called `GetCurrencyConvertorResponse`.
2. Add an **XML to JSON**
    filter to the policy. Configure it to extract the SOAP Body content first and remove any namespaces:

    ![XML to JSON filter](/Images/PolDevGuide/Mapper/xmltojson.png)

3. The GetCurrencyConvertorResponse policy should now look like this:

    ![GetCurrencyConvertorResponse policy](/Images/PolDevGuide/Mapper/GetCurrencyConvertorResponse.png)

4. Add another **Policy Shortcut** filter to the `MainRouter` policy, and set it to call the `GetCurrencyConvertorResponse` policy.

    At this stage, the MainRouter policy should look like this:

    ![Complete MainRouter policy](/Images/PolDevGuide/Mapper/mainrouter_policy_complete.png)

## Test the REST to SOAP mapping

To test the REST to SOAP mapping, deploy the configuration on the API Gateway and send a REST request from a REST client. For example, to get the conversion rate for EUR to USD, send a request to the URL:

```
http://localhost:8080/convertcurrency/getConversionRate?fromCurrency=EUR&toCurrency=USD
```

The following is an example JSON response:

```
{
    "ConversionRateResponse": {
        "ConversionRateResult": 1.1194
    }
}
```

You can use API Gateway Manager to view the filter execution path for the REST request, and to examine the request from the client and the response from API Gateway, and the request from API Gateway and the response from the web service.

![Execution path in API Gateway Manager](/Images/PolDevGuide/Mapper/gw_mgr_filter_path.png)
