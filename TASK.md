# Directions

In the accompanying XML files (`availability/provider-a.xml` and
`availability/provider-b.xml`), you'll find similar data in two disparate
formats. Your task is to ingest these two data files using Clojure and output
them to a single CSV with the following columns:

* property code
* rate plan code
* room type code
* room type description
* currency code
* total before tax
* total after tax
* average nightly rate
* lowest room rate

Please include a header row, using the above as column names. Lowest room rate
is a derived value. Read on for details.

## Deriving the lowest room rate

Often, rooms (signified by room type code) will appear under the same property
multiple times, under different rate plans (signified by rate plan code). Your
task here is to identify the lowest rate for a given room amongst all the rate
plans it is featured on at that hotel (signfied by property code). The value for
the "lowest room rate" column should be "TRUE" for the lowest rate and "FALSE"
for all other rates. Rooms may have the same rate on different rate plans. When
the lowest rate is available under more than one rate plan mark all of them
"TRUE" and any other rates for that room "FALSE".

## Finding the attributes

Since this exercise isn't meant to test powers of interpretation, we've
mapped out (in simplified XPath) the locations of the attributes you'll need
to retrieve by provider below:

### Provider A

<table>
  <tbody>
    <tr>
      <td>property code</td>
      <td><code>/HotelML/Property/@Code</code></td>
    </tr>
    <tr>
      <td>rate plan code</td>
      <td>
        <code>/HotelML/Property/Rate/RatePlan/@Code</code>
        <p>unless there is an attribute at:</p>
        <code>/HotelML/Property/Rate/RatePlan/@RequestedRatePlanCode</code>
        <p>which you should prefer</p>
      </td>
    </tr>
    <tr>
      <td>room type code</td>
      <td>
        <code>/HotelML/Property/Rate/RatePlan/RoomType/@Code</code>
        <p>unless there is an attribute at:</p>
        <code>/HotelML/Property/Rate/RatePlan/RoomType/@StandardRoomType</code>
        <p>which you should prefer</p>
      </td>
    </tr>
		<tr>
      <td>room type description</td>
      <td><code>/HotelML/Property/Rate/RatePlan/RoomType/@RoomDescription</code></td>
    </tr>
    <tr>
      <td>currency code</td>
      <td><code>/HotelML/Property/Rate/Rateplan/RoomType/@NativeCurrency</code></td>
    </tr>
    <tr>
      <td>average nightly rate</td>
      <td><code>/HotelML/Property/Rate/Rateplan/RoomType/@BookableRate</code></td>
    </tr>
    <tr>
      <td>total before tax</td>
      <td><code>/HotelML/Property/Rate/RatePlan/RoomType/@TotalRate</code></td>
    </tr>
    <tr>
      <td>total after tax</td>
      <td><code>/HotelML/Property/Rate/RatePlan/RoomType/@TotalRateInclusive</code></td>
    </tr>
  </tbody>
</table>

### Provider B

<table>
  <tbody>
    <tr>
      <td>property code</td>
      <td><code>/OTA_HotelAvailRS/RoomStays/RoomStay/BasicPropertyInfo/@HotelCode</code></td>
    </tr>
    <tr>
      <td>rate plan code</td>
      <td><code>/OTA_HotelAvailRS/RoomStays/RoomStay/RoomRates/RoomRate/@RatePlanCode</code></td>
    </tr>
    <tr>
      <td>room type code</td>
      <td><code>/OTA_HotelAvailRS/RoomStays/RoomStay/RoomRates/RoomRate/@RoomTypeCode</code></td>
    </tr>
    <tr>
      <td>room type description</td>
      <td><code>/OTA_HotelAvailRS/RoomStays/RoomStay/RoomTypes/RoomType/RoomDescription/Text</code></td>
    </tr>
    <tr>
      <td>currency code</td>
      <td><code>/OTA_HotelAvailRS/RoomStays/RoomStay/RoomRates/RoomRate/Rates/Rate/Total/@CurrencyCode</code></td>
    </tr>
    <tr>
      <td>average nightly rate</td>
      <td><code>/OTA_HotelAvailRS/RoomStays/RoomStay/RoomRates/RoomRate/Rates/Rate/Base/@AmountBeforeTax</code></td>
    </tr>
    <tr>
      <td>total before tax</td>
      <td><code>/OTA_HotelAvailRS/RoomStays/RoomStay/RoomRates/RoomRate/Rates/Rate/Total/@AmountBeforeTax</code></td>
    </tr>
    <tr>
      <td>total after tax</td>
      <td><code>/OTA_HotelAvailRS/RoomStays/RoomStay/RoomRates/RoomRate/Rates/Rate/Total/@AmountAfterTax</code></td>
    </tr>
  </tbody>
</table>

## Submitting your solution

Please return your solution by email as a runnable project in a single zip file. Include any instructions for invoking the function that will emit the CSV file in an accompanying README. Please also avoid any dependencies which cannot be pulled in via Leiningen/Boot/Maven, etc (read: avoid non-clojure/non-java dependencies).

_Take your time, have fun and reach out if you have any questions. THANKS!_
