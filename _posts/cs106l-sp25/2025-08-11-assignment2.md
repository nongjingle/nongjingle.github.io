---
title: "assignment2 of cs106l-sp25"
categories: [cs106l-sp25]
tags: [cs106l-sp25]
---
## assignment2 of cs106l-sp25
```cpp
/*
 * CS106L Assignment 2: Marriage Pact
 * Created by Haven Whitney with modifications by Fabio Ibanez & Jacob
 * Roberts-Baca.
 *
 * Welcome to Assignment 2 of CS106L! Please complete each STUDENT TODO
 * in this file. You do not need to modify any other files.
 *
 */

#include <fstream>
#include <iostream>
#include <queue>
#include <set>
#include <string>
#include <unordered_set>

std::string kYourName = "Charlie Brown";  // Don't forget to change this!

/**
 * Takes in a file name and returns a set containing all of the applicant names
 * as a set.
 *
 * @param filename  The name of the file to read.
 *                  Each line of the file will be a single applicant's name.
 * @returns         A set of all applicant names read from the file.
 *
 * @remark Feel free to change the return type of this function (and the
 * function below it) to use a `std::unordered_set` instead. If you do so, make
 * sure to also change the corresponding functions in `utils.h`.
 */
std::set<std::string> get_applicants(std::string filename) {
  // STUDENT TODO: Implement this function.
  std::set<std::string> names_set{};
  std::ifstream ifs(filename);
  std::string name{};

  while (std::getline(ifs, name)) {
    names_set.insert(name);
  }

  return names_set;
}

/**
 * Takes in a set of student names by reference and returns a queue of names
 * that match the given student name.
 *
 * @param name      The returned queue of names should have the same initials as
 * this name.
 * @param students  The set of student names.
 * @return          A queue containing pointers to each matching name.
 */
std::queue<const std::string*> find_matches(
    std::string name, const std::set<std::string>& students) {
  // STUDENT TODO: Implement this function.
  std::queue<const std::string*> matches{};

  auto get_initials = [](std::string name) {
    std::string initials{};
    initials += name[0];
    size_t i = 1;
    while (name[i] != '\x20') {
      ++i;
    }
    initials += name[i + 1];
    return initials;
  };

  std::string my_initials = get_initials(kYourName);
  std::string initials{};

  for (const auto& name : students) {
    initials = get_initials(name);
    if (initials == my_initials) matches.push(&name);
  }

  return matches;
}

/**
 * Takes in a queue of pointers to possible matches and determines the one true
 * match!
 *
 * You can implement this function however you'd like, but try to do something a
 * bit more complicated than a simple `pop()`.
 *
 * @param matches The queue of possible matches.
 * @return        Your magical one true love.
 *                Will return "NO MATCHES FOUND." if `matches` is empty.
 */
std::string get_match(std::queue<const std::string*>& matches) {
  // STUDENT TODO: Implement this function.
  if (matches.empty()) return "NO MATCHES FOUND.";
  std::string best_match = *matches.front();
  matches.pop();

  while (!matches.empty()) {
    std::string curr = *matches.front();
    matches.pop();
    if (curr < best_match) best_match = curr;
  }

  return best_match;
}

/* #### Please don't remove this line! #### */
#include "autograder/utils.hpp"

```
