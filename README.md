# C++ coding

Attribute Parser


#include <iostream>
#include <vector>
#include <string>
#include <map>
#include <sstream>

using namespace std;

int main() {
    int n, q;
    cin >> n >> q;
    cin.ignore(); // Consume newline after q

    map<string, string> hrml_map;
    vector<string> tag_stack;

    for (int i = 0; i < n; i++) {
        string line;
        getline(cin, line);
        
        // Remove brackets
        line.erase(0, 1); // Remove '<'
        if (line[0] == '/') {
            // Closing tag: pop from stack
            tag_stack.pop_back();
        } else {
            // Opening tag: parse name and attributes
            line.pop_back(); // Remove '>'
            stringstream ss(line);
            string tag_name, attr_name, equals, attr_value;
            
            ss >> tag_name;
            tag_stack.push_back(tag_name);
            
            // Build current path (e.g., tag1.tag2)
            string current_path = "";
            for (size_t j = 0; j < tag_stack.size(); j++) {
                current_path += (j == 0 ? "" : ".") + tag_stack[j];
            }
            
            // Extract attributes
            while (ss >> attr_name >> equals >> attr_value) {
                // Remove quotes from "value"
                if (attr_value.front() == '\"') attr_value.erase(0, 1);
                if (attr_value.back() == '\"') attr_value.pop_back();
                
                // Store in map as path~attribute
                hrml_map[current_path + "~" + attr_name] = attr_value;
            }
        }
    }

    // Handle Queries
    for (int i = 0; i < q; i++) {
        string query;
        getline(cin, query);
        if (hrml_map.count(query)) {
            cout << hrml_map[query] << endl;
        } else {
            cout << "Not Found!" << endl;
        }
    }

    return 0;
}


#Classes and objects


#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
#include <cassert>
using namespace std;
// Write your Student class here
class Student {
    private:
        int scores[5];
    
    public:
        void input() {
            for (int i = 0; i < 5; i++) {
                cin >> scores[i];
            }
        }
        
 int calculateTotalScore() {
            int sum = 0;
            for (int& score : scores) {
                sum += score;
            }
            return sum;
        }
};

int main() {
    int n; // number of students
    cin >> n;
    Student *s = new Student[n]; // an array of n students
    
    for(int i = 0; i < n; i++){
        s[i].input();
    }

    // calculate kristen's score
    int kristen_score = s[0].calculateTotalScore();

    // determine how many students scored higher than kristen
    int count = 0; 
    for(int i = 1; i < n; i++){
        int total = s[i].calculateTotalScore();
        if(total > kristen_score){
            count++;
        }
    }

    // print result
    cout << count;
    
    return 0;
}


#structs


#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
#include<string>
using namespace std;

/*
    add code for struct here.
*/

struct Student{
    int age;
    string first_name;
    string last_name;
    string standard;
};
int main() {
    Student st;
    
    cin >> st.age >> st.first_name >> st.last_name >> st.standard;
    cout << st.age << " " << st.first_name << " " << st.last_name << " " << st.standard;
    
    return 0;
}


#vector sort

#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
using namespace std;


int main() {
    int n, temp;
    cin >> n; // Read the number of elements
    
    vector<int> v;
    for(int i = 0; i < n; i++) {
        cin >> temp;
        v.push_back(temp); // Add each number to the vector
    }
    
    // Sort the vector in ascending order
    sort(v.begin(), v.end());
    
    // Print each element followed by a space
    for(int i = 0; i < v.size(); i++) {
        cout << v[i] << " ";
    }
    
    return 0;
}


#vector erase

#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
using namespace std;


#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
    int n;
    cin >> n;
    
    vector<int> v(n);
    for(int i = 0; i < n; i++) {
        cin >> v[i];
    }

    // First query: Remove element at a specific position
    int pos;
    cin >> pos;
    v.erase(v.begin() + (pos - 1));

    // Second query: Remove elements in a range [a, b)
    int a, b;
    cin >> a >> b;
    // a-1 to reach the 0-based index; b-1 because the erase function is already exclusive of the end
    v.erase(v.begin() + (a - 1), v.begin() + (b - 1));

    // Output the final size and elements
    cout << v.size() << endl;
    for(int i = 0; i < v.size(); i++) {
        cout << v[i] << " ";
    }

    return 0;
}


#median of two sorted array

class Solution {
public:
 double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    int m = nums1.size(), n = nums2.size();
    int total = m + n;
    int i = 0, j = 0;
    int m1 = 0, m2 = 0; // To store the middle values

    for (int count = 0; count <= total / 2; count++) {
        m2 = m1; // Store the previous value for even cases
        if (i < m && (j >= n || nums1[i] < nums2[j])) {
            m1 = nums1[i++];
        } else {
            m1 = nums2[j++];
        }
    }

    if (total % 2 == 0) return (m1 + m2) / 2.0;
    return m1;
}

};



 Longest Substring Without Repeating Characters


#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        // Use an array for ASCII characters (128 total) to store last seen index
        vector<int> charMap(128, -1);
        int maxLength = 0;
        int left = 0; // Left boundary of our sliding window

        for (int right = 0; right < s.length(); right++) {
            // If the character was seen before AND is inside our current window
            if (charMap[s[right]] >= left) {
                left = charMap[s[right]] + 1;
            }

            // Update the last seen position of the character
            charMap[s[right]] = right;
            
            // Calculate current window size and update max
            maxLength = max(maxLength, right - left + 1);
        }

        return maxLength;
    }
};


palindrome 

class Solution {
public:
    bool isPalindrome(int x) {
        int temp1=x;
        int reverse=0;
        if(x<0){
            return false;
        }
        while(x!=0){
            reverse=(reverse*10)+(x%10);
            x=x/10;
        }
        return (reverse==temp1);
        
    }
};

Regular Expression Matching

#include <vector>
#include <string>

using namespace std;

class Solution {
public:
    bool isMatch(string s, string p) {
        int m = s.length(), n = p.length();
        // dp[i][j] will be true if s[0..i-1] matches p[0..j-1]
        vector<vector<bool>> dp(m + 1, vector<bool>(n + 1, false));
        
        dp[0][0] = true; // Empty string matches empty pattern

        // Pre-fill for patterns like "a*", "a*b*", ".*" that can match an empty string
        for (int j = 2; j <= n; j++) {
            if (p[j - 1] == '*') {
                dp[0][j] = dp[0][j - 2];
            }
        }

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (p[j - 1] == '.' || p[j - 1] == s[i - 1]) {
                    // Direct match or wildcard '.'
                    dp[i][j] = dp[i - 1][j - 1];
                } else if (p[j - 1] == '*') {
                    // Case 1: Treat '*' as zero of the preceding element
                    dp[i][j] = dp[i][j - 2];
                    
                    // Case 2: If preceding element matches current string char
                    if (p[j - 2] == '.' || p[j - 2] == s[i - 1]) {
                        dp[i][j] = dp[i][j] || dp[i - 1][j];
                    }
                }
            }
        }

        return dp[m][n];
    }
};
