# Consolidation\Comments

A tool for preserving comments, e.g. when parsing YAML files.

[![Build Status](https://travis-ci.org/T2L/comments.svg?branch=master)](https://travis-ci.org/T2L/comments)
[![codecov](https://codecov.io/gh/T2L/comments/branch/master/graph/badge.svg)](https://codecov.io/gh/T2L/comments)

## Component Status

Prototype.

## Motivation

Do lightweight editing on YAML files and then rewrite the file without losing embedded comments.

## Usage
```
// First step: read the file, parse the yaml, edit and dump the results.

$original_contents = file_get_contents($filepath);
$parsed_data = Yaml::parse($original_contents);
$processed_data = $this->my_processing_function($parsed_data);
$altered_contents = Yaml::dump($parsed_data, PHP_INT_MAX, 2, Yaml::DUMP_MULTI_LINE_LITERAL_BLOCK);

// Second step: collect comments from original document and inject them into result.

$commentManager = new Comments();
$commentManager->collect(explode("\n", $original_contents));
$altered_with_comments = $commentManager->inject(explode("\n", $altered_contents));

$result = implode("\n", $altered_with_comments);
```
## Limitations

The comment manager collects groups of comment lines and associates them with the first non-blank content line that follows the comment. If there are multiple non-blank content lines that are exactly the same, then the comment lines are re-injected in the same order they appeared in the original file.

## Installation

$ composer require consolidation/comments

## Comparison to Existing Solutions

See Klausi's [yaml_comments](https://github.com/klausi/yaml_comments).
