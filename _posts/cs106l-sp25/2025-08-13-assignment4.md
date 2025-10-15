---
title: "assignment4 of cs106l-sp25"
categories: [cs106l-sp25]
tags: [cs106l-sp25]
---
## assignment4 of cs106l-sp25
```cpp
#include "spellcheck.h"

#include <algorithm>
#include <cctype>
#include <iostream>
#include <numeric>
#include <ranges>
#include <set>
#include <vector>

template <typename Iterator, typename UnaryPred>
std::vector<Iterator> find_all(Iterator begin, Iterator end, UnaryPred pred);

Corpus tokenize(std::string& source) {
  /* TODO: Implement this method */
  auto space_it_vec = find_all(source.begin(), source.end(),
                               [](char ch) { return std::isspace(ch); });

  std::vector<Token> tokens{};

  std::transform(space_it_vec.begin(), std::prev(space_it_vec.end()),
                 space_it_vec.begin() + 1, std::inserter(tokens, tokens.end()),
                 [&source](const auto& begin, const auto& end) {
                   return Token(source, begin, end);
                 });

  std::erase_if(tokens,
                [](const Token& token) { return token.content.empty(); });

  return Corpus(tokens.begin(), tokens.end());
}

std::set<Misspelling> spellcheck(const Corpus& source,
                                 const Dictionary& dictionary) {
  /* TODO: Implement this method */
  auto find_error = [&dictionary](const Token& token) {
    return !dictionary.contains(token.content);
  };

  auto set_misspelling = [&dictionary](const Token& token) -> Misspelling {
    auto view = dictionary |
                std::ranges::views::filter([&token](const std::string& str) {
                  return levenshtein(str, token.content) == 1;
                });
    std::set<std::string> suggestions(view.begin(), view.end());

    return Misspelling{token, suggestions};
  };

  auto view = source | std::ranges::views::filter(find_error) |
              std::ranges::views::transform(set_misspelling) |
              std::ranges::views::filter([](const Misspelling& misspelling) {
                return !misspelling.suggestions.empty();
              });

  return std::set<Misspelling>(view.begin(), view.end());
}

/* Helper methods */

#include "utils.cpp"
```
