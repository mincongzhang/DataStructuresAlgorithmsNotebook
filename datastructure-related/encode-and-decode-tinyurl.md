### Encode and Decode TinyURL

TinyURL is a URL shortening service where you enter a URL such as https://leetcode.com/problems/design-tinyurl and it returns a short URL such as http://tinyurl.com/4e9iAk.

Design the encode and decode methods for the TinyURL service. There is no restriction on how your encode/decode algorithm should work. You just need to ensure that a URL can be encoded to a tiny URL and the tiny URL can be decoded to the original URL.

https://leetcode.com/problems/encode-and-decode-tinyurl/#/description

```
#include <sstream>
#include <unordered_set> //hash

namespace {
    std::string toString(unsigned int num){
        stringstream ss;
        ss<<num;
        return ss.str();
    }
}

class Solution {
private:
    std::unordered_map<std::string,std::string> m_hash;

public:
    // Encodes a URL to a shortened URL.
    string encode(string longUrl) {
        unsigned int num = std::hash<std::string>{}(longUrl);
        std::string key = toString(num);
        m_hash[key] = longUrl;
        return key;
    }

    // Decodes a shortened URL to its original URL.
    string decode(string shortUrl) {
        return m_hash[shortUrl];
    }
};

// Your Solution object will be instantiated and called as such:
// Solution solution;
// solution.decode(solution.encode(url));
```