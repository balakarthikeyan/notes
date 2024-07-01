Mocha supports more traditional TDD interfaces:

suite: analogous to describe
test: analogous to it
setup: analogous to before
teardown: analogous to after
suiteSetup: analogous to beforeEach
suiteTeardown: analogous to afterEach

TDD with the Assert
var assert = require('chai').assert;

BDD with the Except
var expect = require('chai').expect;