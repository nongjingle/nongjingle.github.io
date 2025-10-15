---
title: "assignment5 of cs106l-sp25"
categories: [cs106l-sp25]
tags: [cs106l-sp25]
---
## assignment5 of cs106l-sp25

The following is the source code of `user.h`:
```cpp
#pragma once
/*
 * CS106L Assignment 5: TreeBook
 * Created by Fabio Ibanez with modifications by Jacob Roberts-Baca.
 */

#include <iostream>
#include <string>

class User {
 public:
  User(const std::string& name);
  void add_friend(const std::string& name);
  std::string get_name() const;
  size_t size() const;
  void set_friend(size_t index, const std::string& name);

  /**
   * STUDENT TODO:
   * Your custom operators and special member functions will go here!
   *
   */
  friend std::ostream& operator<<(std::ostream& os, const User& user);

  ~User();

  User(const User& user);

  User& operator=(const User& user);

  User(User&& user) = delete;
  User& operator=(User&& user) = delete;

  User& operator+=(User& user);

  bool operator<(const User& user);

 private:
  std::string _name;
  std::string* _friends;
  size_t _size;
  size_t _capacity;
};
```

The following is the source code of `user.cpp`:
```cpp
#include "user.h"

/**
 * Creates a new User with the given name and no friends.
 */
User::User(const std::string& name)
    : _name(name), _friends(nullptr), _size(0), _capacity(0) {}

/**
 * Adds a friend to this User's list of friends.
 * @param name The name of the friend to add.
 */
void User::add_friend(const std::string& name) {
  if (_size == _capacity) {
    _capacity = 2 * _capacity + 1;
    std::string* new_friends = new std::string[_capacity];
    for (size_t i = 0; i < _size; ++i) {
      new_friends[i] = _friends[i];
    }
    new_friends[_size] = name;
    ++_size;
    delete[] _friends;
    _friends = new_friends;
  }
  _friends[_size++] = name;
}

/**
 * Returns the name of this User.
 */
std::string User::get_name() const { return _name; }

/**
 * Returns the number of friends this User has.
 */
size_t User::size() const { return _size; }

/**
 * Sets the friend at the given index to the given name.
 * @param index The index of the friend to set.
 * @param name The name to set the friend to.
 */
void User::set_friend(size_t index, const std::string& name) {
  _friends[index] = name;
}

/**
 * STUDENT TODO:
 * The definitions for your custom operators and special member functions will
 * go here!
 */

std::ostream& operator<<(std::ostream& os, const User& user) {
  os << "User(name=" << user.get_name() << ", friends=[";

  size_t friend_size = user.size();

  for (size_t i = 0; i < friend_size; ++i) {
    os << user._friends[i];
    if (i + 1 > friend_size) os << ", ";
  }

  os << "])";

  return os;
}

User::~User() { delete[] _friends; }

User::User(const User& user)
    : _name(user._name), _size(user._size), _capacity(user._capacity) {
  if (_capacity > 0) {
    std::string* _friends = new std::string[_capacity];
    for (size_t i = 0; i < _size; ++i) {
      _friends[i] = user._friends[i];
    }
  } else {
    _friends = nullptr;
  }
}

User& User::operator=(const User& user) {
  if (this == &user) return *this;

  if (_capacity != 0) {
    delete[] _friends;
  }

  _name = user._name;
  _size = user._size;
  _capacity = user._capacity;

  if (_capacity > 0) {
    std::string* _friends = new std::string[_capacity];
    for (size_t i = 0; i < _size; ++i) {
      _friends[i] = user._friends[i];
    }
  } else {
    _friends = nullptr;
  }

  return *this;
}

User& User::operator+=(User& user) {
  add_friend(user.get_name());
  user.add_friend(get_name());
}

bool User::operator<(const User& user) { return get_name() < user.get_name(); }
```
