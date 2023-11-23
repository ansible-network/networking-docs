We follow [Ansible Code of Conduct](https://docs.ansible.com/ansible/latest/community/code_of_conduct.html) in all our contributions and interactions within this repository.

## Issue Tracker and Open Pull Requests

Whether you're eager to make a meaningful contribution or have encountered a bug you're ready to fix, we encourage you to visit our issue tracker. There, you'll discover opportunities to implement new features, address reported bugs, or even initiate a discussion by submitting an issue to refine your ideas before embarking on your work. This initial step can greatly assist in choosing the right direction for your efforts, potentially saving you time and energy. Furthermore, it's worth checking the list of open pull requests before tackling an issue, as someone might already be in the process of addressing it. This collaboration can lead to more effective and efficient solutions.

## Our workflow on GitHub

Our team policies are largely shaped by our workflow. Please make sure you are familiar with the following workflow steps before opening a pull request:

- Start by forking the repository relevant to your work, creating a personal repository.
- Develop your changes within the designated branch for your commits.
- Open a Pull Request back to the main collection repository and ensure it includes a clear explanation of the changes made and why they are being made.
- Patiently await reviews and be prepared to make any required code modifications based on feedback.
- Seek a final review from one of the repository maintainers.

## Contributing to modules

Contributors should maintain well-organized code with clear comments to enhance readability and maintain our project's quality standards. Furthermore, it's crucial to validate that all tests pass before requesting a review. If you're adding new functionality, remember to include tests to verify its proper operation. For more details on testing, please refer to the testing section.

### Modifying `argspec`

When your pull request modifies the `argspec`, it's important to provide clear reasons for the changes and follow specific rules:

#### Rule 1: Deprecating Unused Attributes

If the change renders a certain attribute unnecessary, it should be marked for deprecation instead of immediate removal, in accordance with our deprecation process.

#### Rule 2: Changing Attribute Behavior

If you wish to alter the behavior of an attribute, create a new attribute to perform the desired operation and deprecate the older attribute.

### Deprecation Process for Attributes

When an attribute needs to be deprecated, follow these steps:

1. **Deprecation Notice**: In the attribute's description, include a deprecation notice similar to the following: "This option has been deprecated and will be removed in a release occurring at least 2 years from the current date."
2. **Documentation**: Add documentation for the new attribute.
3. **Facts and Config Support**: Implement facts and configuration support for the new attribute. Continue rendering both the old and new keys in facts until the removal date. However, update example plays to exclusively use the new attribute.
4. **Transformation Handling**: Ensure that facts only render the new attribute. If someone adds the old attribute via a playbook, the configuration code should handle the transformation accordingly.

This approach ensures a smooth transition when deprecating attributes in our codebase.

## Testing Pull Requests: Running Local Unit or Integration Tests

Prior to merging a pull request, it is vital to confirm the successful completion of all Continuous Integration (CI) checks, encompassing unit tests. Additionally, contributors should run both local unit and integration tests to verify the functionality of the changes on the respective appliance. Below is the short version for running the tests:

- test-requirements.txt

```
pytest                                   7.1.2
pytest-ansible-units                     0.1.dev61
pytest-cov                               4.0.0
pytest-forked                            1.4.0
pytest-xdist                             2.5.0
```

- We will be using `pytest` for running unit tests:

```sh
pytest -vvv --log-level WARNING --color yes ./tests
```

- We will be using `ansible-test` for running the integration tests:

```sh
ansible-test network-integration --python <python_version> --inventory <inventory_file> <module_name> -vvv
```
