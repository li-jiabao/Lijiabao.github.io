
# Set up the Policy File to Grant the Required Permissions

This step uses the Policy Tool utility to open the policy file named **`examplepolicy`** created in the 
[Creating a Policy File](../tour1/index.html) lesson. You will add a new policy entry granting permission for code from the directory where `GetProps.class` is stored to read the `"user.home"` and the `"java.home"` property values, as shown in the following figure.

The steps are as follows.

1. [Open the Policy File](wstep1.html)
1. [Grant the Required Permissions](wstep2.html)
1. [Save the Policy File](wstep3.html)

- You retrieve the `examplepolicy` file from the `test` directory in your home directory.
- For the **CodeBase** URL in the step for granting the required permissions, you can substitute `file:${user.home}/test/` for `file:/C:/Test/`. Alternatively you could directly specify your home directory rather than referring to the `"user.home"` property, as in `file:/home/jones/test/`.
