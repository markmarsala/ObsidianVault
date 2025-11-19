- Altering parameter values
- Modifying request headers
- Changing the order of parameters
- Introducing unexpected data types or formats

1. Parameter Fuzzing
2. Data Format Fuzzing
3. Sequence Fuzzing

## Fuzzing the API
```
git clone https://github.com/PandaSt0rm/webfuzz_api.git
cd webfuzz_api
pip3 install -r requirements.txt
python3 api_fuzzer.py http://10.10.15.141:50324
curl http://83.136.255.106:52831/czcmdcvt
```
