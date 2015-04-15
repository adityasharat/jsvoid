> How would you convert a number to words?

Today someone asked me this question, and I thought lets just try to implemnt it. So here is the problem statement.

Write a function/method that takes a <code class="jsv-fs-inherit">number</code> as an input and returns a <code class="jsv-fs-inherit">string</code> that represents the number in words. For example

```language-javascript
var number = 12345;
inWords(number); // Twelve Thousand One Hundred Thirty Five
```

##### So here is my solution

```language-javascript
(function () {
    function toWords() {
        var ones,
            tens,
            hundred,
            output,
            numString,
            denominations,
            groups,
            number = this;

        if (number && number.valueOf && typeof number.valueOf() !== 'number') {
            throw new Error('Input must be a number');
        }

        if (number.valueOf() === 0) {
            return 'Zero';
        }

        function reverse(s) {
            var i, o = '';
            for (i = s.length - 1; i >= 0; i--) {
                o += s[i];
            }
            return o;
        }

        ones = ['', ' One', ' Two', ' Three', ' Four', ' Five', ' Six', ' Seven', ' Eight', ' Nine', ' Ten', ' Eleven', ' Twelve', ' Thirteen', ' Fourteen', ' Fifteen', ' Sixteen', ' Seventeen', ' Eighteen', ' Nineteen'];
        tens = ['', '', ' Twenty', ' Thirty', ' Forty', ' Fifty', ' Sixty', ' Seventy', ' Eighty', ' Ninety'];
        hundred = ' Hundred';
        denominations = ['', '', ' Thousand', ' Million', ' Billion', ' Trillion'];
        numString = parseInt(number.valueOf(), 10).toString().replace('-', '');
        groups = reverse(numString).match(/.{1,3}/g).map(reverse).reverse();
        output = '';

        function convert3DigitNumberToWord(numberString) {
            var result = '',
                _3DigitNumber = parseInt(numberString, 10),
                _3DigitNumberString = _3DigitNumber.toString(),
                tenth;

            if (_3DigitNumber < 20) {
                result = ones[_3DigitNumber];
            } else if (_3DigitNumberString.length === 3) {
                result = ones[parseInt(_3DigitNumberString.charAt(0), 10)] + hundred;
                tenth = parseInt(_3DigitNumberString.charAt(1) + _3DigitNumberString.charAt(2), 10);
                if (tenth < 20) {
                    result += ones[tenth];
                } else {
                    result += tens[parseInt(_3DigitNumberString.charAt(1), 10)];
                    result += ones[parseInt(_3DigitNumberString.charAt(2), 10)];
                }
            } else {
                result += tens[parseInt(_3DigitNumberString.charAt(0), 10)];
                result += ones[parseInt(_3DigitNumberString.charAt(1), 10)];
            }

            return result;
        }

        groups = groups.forEach(function (_3DigitNumberString, index, groups) {
            output += convert3DigitNumberToWord(_3DigitNumberString) + denominations[groups.length - index];
        });

        if (number < 0) {
            output = 'Negative' + output;
        }

        return output.trim();
    }

    Number.prototype.toWords = toWords;
}());
```

###### Usage

```language-javascript
var number = 2359813;
number.toWords();
// "Two Million Three Hundred Fifty Nine Thousand Eight Hundred Thirteen"
```

Ofcource we could have used some complex <code class="jsv-fs-inherit">RegExp</code> to bring the code down to 12-15 lines. But that's no fun. :P

I haven't added support for floating point numbers just yet,  that's coming soon.

Please let me know if I missed out any corner cases or if you know a better solution to this problem.
