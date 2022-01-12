# prerelease-number
GitHub action for generating sequential prerelease numbers. The prerelease number is stored in your GitHub repository as a ref, it doesn't add any extra commits to your repository. Use in your workflow like so:

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Generate prerelease number
      uses: hdimon/prerelease-number@v1
      with:
        token: ${{secrets.github_token}}
        prefix: 'master'
        prerelease_type: 'alpha'
    - name: Print new prerelease number
      run: echo "Prerelease number is $PRERELEASE_NUMBER"
      # Or, if you're on Windows: echo "Prerelease number is ${env:PRERELEASE_NUMBER}"
```

## How it works

It generates incremental number which is unique inside "prefix-prerelease_type" pair. For example if prefix = 'master' and prerelease_type = 'alpha' then first call of action will generate PRERELEASE_NUMBER = 1 which you could then concatenate to get "master-alpha-1". The second call of action with the same input parameters on another commit will give PRERELEASE_NUMBER = 2.

**Note:** If then you call the action with prefix = 'master' and prerelease_type = 'beta' on **the same commit** then it will generate **PRERELEASE_NUMBER = 1** because no numbers for "master-beta" pair exists yet.

Thanks for "prefix" parameter it becomes possible to manage releases in multi versional development (thus you can parallelly release 1.1.0-alpha-1 and 2.1.0-alpha-1 for example). You can find example of such usage in [https://github.com/hdimon/SmartHomeApi](https://github.com/hdimon/SmartHomeApi) repository.


This action is inspired and based on [Build Number Generator](https://github.com/einaregilsson/build-number) by [Einar Egilsson](https://github.com/einaregilsson). Thank you very much!
