# Binary reader-writer
C++17 header-only helper class for reading & writing binary data into a std::vector container.

## Working with primitives:
```cpp
#include "buffer.hpp"

int main() {
  buffer buff;
  //works with any unsigned/signed numbers
  buff.write<int16_t>(55);
  buff.write<int32_t>(67);
  buff.write<int64_t>(78);
  buff.write<uint16_t>(52);
  buff.write<uint32_t>(63);
  buff.write<uint64_t>(73);
  buff.reset(); //don't forget to reset when done writing!

  printf("%i\n", buff.read<int16_t>());
  printf("%i\n", buff.read<int32_t>());
  printf("%i\n", buff.read<int64_t>());
  printf("%i\n", buff.read<uint16_t>());
  printf("%i\n", buff.read<uint32_t>());
  printf("%i\n", buff.read<uint64_t>());
  buff.reset(); //same case for reading!
  
  //retrieving vector of bytes
  std::vector<uint8_t> data = buff.data();
  
  //clearing buffer
  buff.clear();

  //you can also restore a buffer
  buff.set(data);
  
  //and the list goes on...
}
```

## Working with strings & containers:
```cpp
#include "buffer.hpp"

int main() {
  buffer buff;

  /* supports std::string */
  buff.write<std::string>("Hello world!");
  
  std::vector<std::string> fancy = {
    "I",
    "Am",
    "Feeling",
    "Nice"
  };
  
  //supports writing containers like std::vector
  buff.write<std::vector<std::string>>(fancy);
  
  std::map<std::string, int16_t> people = {
    {"Alice", 24},
    {"John",  34},
    {"Jeb",   27}
  };

  //supports writing containers like std::map
  buff.write<std::map<std::string, int16_t>>(people);
  
  std::map<int32_t, std::vector<int16_t>> some_list = {
    {1, {1, 2, 3, 4, 5}},
    {6, {1, 2, 3, 4, 5}},
    {7, {1, 2, 3, 4, 5}},
  };

  //you can even write recursively!
  buff.write<std::map<int32_t, std::vector<int16_t>>>(some_list);
  buff.reset();

  printf("%s\n", buff.read<std::string>().c_str());
  
  std::vector<std::string> fancy_backed = buff.read<std::vector<std::string>>();
 
  for(std::string& str : fancy_backed){
    printf("%s\n", str .c_str());
  }

  std::map<std::string, int16_t> people_backed = buff.read<std::map<std::string, int16_t>>();

  for (auto const& [name, age] : people_backed) {
    printf("%s: %i\n", name.c_str(), age);
  }
  
  //and the list goes on...
}
```

## Working with endianness

```cpp
#include "buffer.hpp"

int main() {
  buffer buff;
  
  //if for whatever reason you need to swap endianess just use these flags
  buff.write_swap_endianness = true;
  buff.read_swap_endianness = true;
}
```

## Working with files
```cpp
#include "buffer.hpp"

int main() {
  file_buffer buff;
  
  //loads a buffer from a file
  buff.load("content.txt");
  
  //file_buffer extends buffer so you can do anything with it that a buffer can do.
 
 //save the buffer to a file
 buff.save("content.txt");
}
```

Originally inspired from: [https://github.com/m-byte918/Binary-Reader-Writer](https://github.com/m-byte918/Binary-Reader-Writer).
