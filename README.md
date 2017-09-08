json11
------

## Changes from the original
Support for int64 and uint64 was added.
To save memory, the value in the signed int range is converted to JsonInt (32 bits).
Values that exceed the range of signed int are converted to JsonInt64, JsonUInt64 (64 bits).

```cpp
const std::string int64JsonStr = R"({
"int_max": 2147483647,
"int_min": -2147483648,
"uint_max": 4294967295,
"uint_min": 0,
"int64_max":  9223372036854775807,
"int64_min": -9223372036854775808,
"uint64_max": 18446744073709551615,
"uint64_min": 0
})";

std::string err;
Json int64Json = Json::parse(int64JsonStr, err);
std::cout << "int64Json: " << int64Json.dump() << "\n";
assert(int64Json.dump() == "{\"int64_max\": 9223372036854775807, \"int64_min\": -9223372036854775808, \"int_max\": 2147483647, \"int_min\": -2147483648, \"uint64_max\": 18446744073709551615, \"uint64_min\": 0, \"uint_max\": 4294967295, \"uint_min\": 0}");
assert(int64Json["int_max"].int_value() == INT_MAX);
assert(int64Json["int_min"].int_value() == INT_MIN);
assert(int64Json["uint_max"].uint64_value() == UINT_MAX);
assert(int64Json["uint_min"].uint64_value() == 0);
assert(int64Json["int64_max"].int64_value() == INT64_MAX);
assert(int64Json["int64_min"].int64_value() == INT64_MIN);
assert(int64Json["uint64_max"].uint64_value() == UINT64_MAX);
assert(int64Json["uint64_min"].uint64_value() == 0);
```

## end

json11 is a tiny JSON library for C++11, providing JSON parsing and serialization.

The core object provided by the library is json11::Json. A Json object represents any JSON
value: null, bool, number (int or double), string (std::string), array (std::vector), or
object (std::map).

Json objects act like values. They can be assigned, copied, moved, compared for equality or
order, and so on. There are also helper methods Json::dump, to serialize a Json to a string, and
Json::parse (static) to parse a std::string as a Json object.

It's easy to make a JSON object with C++11's new initializer syntax:

    Json my_json = Json::object {
        { "key1", "value1" },
        { "key2", false },
        { "key3", Json::array { 1, 2, 3 } },
    };
    std::string json_str = my_json.dump();

There are also implicit constructors that allow standard and user-defined types to be
automatically converted to JSON. For example:

    class Point {
    public:
        int x;
        int y;
        Point (int x, int y) : x(x), y(y) {}
        Json to_json() const { return Json::array { x, y }; }
    };

    std::vector<Point> points = { { 1, 2 }, { 10, 20 }, { 100, 200 } };
    std::string points_json = Json(points).dump();

JSON values can have their values queried and inspected:

    Json json = Json::array { Json::object { { "k", "v" } } };
    std::string str = json[0]["k"].string_value();

More documentation is still to come. For now, see json11.hpp.
