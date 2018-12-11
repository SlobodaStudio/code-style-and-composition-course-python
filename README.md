# Code style and composition course in Python
Code style and composition course for junior ML developers (in Python)

## Test task

### Input

You can find the `input.txt` file at the root of the repository. It contains several records each of them representing a tweet body and a JSON-encoded tags array (a sample of 3 lines is presented):

```txt
$ABBV why price is going down, despite good results?,['@price']
$CMA max pain is 87.5 for expiry 2018-11-16 Source: http://sweep.ly/maxpain.html,['@source']
#STAAnalystAlert for $BLL : KeyCorp Reiterates with a rating of Hold. Our own verdict is Strong Buy http://www.stocktargetadvisor.com/toprating,['@keycorp']
```

Your job is to write a Python script that will tidy up this file according to a set of rules.

### Processing rules

For the tags array:

1. Remove quotes and square brackets for each of the text tokens
2. Place all cleaned-up tags into a separate array for each tweet's resulting record (under the `metadata` key)


For the text:

1. Remove words starting with `$` sign
2. Place all words starting with `@` or `#` to a separate array for each tweet's resulting record (`body_tags`)
3. If a tweet body contains a URL, add it to the array holding the cleaned-up tags (`metadata`)
4. Tokenize the tweet body: just separate by whitespace and remove all punctuation signs at the end of the tokens or in the middle of a whitespace (`why?` -> `why`, remove ` : `). Skip all tokens starting with `$`, `@`, `#` and URLs. Check the rest of the tokens for inclusion in Wordnet corpus using Lesk algorithm from `nltk.wsd`. Place all tokens that are not included into the corpus into a separate array under the `orphan_tokens` key for each tweet's resulting record.

### Output

The full output of the script should be an `output.json` file created at the root of the project overwriting any existing file with the same name. The output JSON in this file should be of the following structure:

```json5
{
  "records": [
    {
      "body": "...", // the entire tweet body, untouched
      "body_tags": [], // array of Strings that are your body tags (#this or @this)
      "metadata": [], // array of Strings that are your cleaned-up up tags from the tags field
      "orphan_tokens": [], // array of Strings that are your tokens that are missing from the words corpus
    }
  ]
}
```

For a sample of input given [above](#Input), the output fragment should look like the following:

```json5
{
  "records": [
    {
      "body": "$ABBV why price is going down, despite good results?",
      "body_tags": [],
      "metadata": ["price"],
      "orphan_tokens": []
    },
    {
      "body": "$CMA max pain is 87.5 for expiry 2018-11-16 Source: http://sweep.ly/maxpain.html",
      "body_tags": [],
      "metadata": ["source", "http://sweep.ly/maxpain.html"],
      "orphan_tokens": ["87.5", "for", "2018-11-16", "Source:"]
    },
    {
      "body": "#STAAnalystAlert for $BLL : KeyCorp Reiterates with a rating of Hold. Our own verdict is Strong Buy http://www.stocktargetadvisor.com/toprating",
      "body_tags": ["STAAnalystAlert"],
      "metadata": ["keycorp", "http://www.stocktargetadvisor.com/toprating"],
      "orphan_tokens": ["for", "KeyCorp", "with", "of", "our"]
    }
  ]
}
```

## The process

1. Make a [public fork](https://help.github.com/articles/fork-a-repo/) of this repo into your Github account
2. Add your commits to the `master` branch of your fork
3. Make [a pull request](https://help.github.com/articles/creating-a-pull-request-from-a-fork/) from your `master` to the [source repo's](https://github.com/SlobodaStudio/code-style-and-composition-course-python/) `master` branch.
4. Let's discuss your work!
