composer require --dev behat/behat
composer require --dev friends-of-behat/symfony-extension
vendor/bin/behat --init
vendor/bin/behat --story-syntax
vendor/bin/behat --append-snippets
vendor/bin/behat test_ls

[Business Need|Ability|Feature]: Internal operations
  In order to stay secret
  As a secret organization
  We need to be able to erase past agents' memory

  Background:
    [Given|*] there is agent A
    [And|*] there is agent B

  [Scenario|Example]: Erasing agent memory
    [Given|*] there is agent J
    [And|*] there is agent K
    [When|*] I erase agent K's memory
    [Then|*] there should be agent J
    [But|*] there should not be agent K

  [Scenario Template|Scenario Outline]: Erasing other agents' memory
    [Given|*] there is agent <agent1>
    [And|*] there is agent <agent2>
    [When|*] I erase agent <agent2>'s memory
    [Then|*] there should be agent <agent1>
    [But|*] there should not be agent <agent2>

    [Scenarios|Examples]:
      | agent1 | agent2 |
      | D      | M      |