<!-- deleted by customization * Open **QSTodoListViewController.m** and add the following method. Change _facebook_ to _microsoftaccount_, _twitter_, _google_, or _windowsazureactivedirectory_ if you're not using Facebook as your identity provider. -->
<!-- keep by customization: begin -->

 Open the project file QSTodoListViewController.m and in the **viewDidLoad** method, remove the following code that reloads the data into the table:
<!-- keep by customization: end -->

```
        - (void) loginAndGetData
        {
            MSClient *client = self.todoService.client;
            if (client.currentUser != nil) {
                return;
            }
            [client loginWithProvider:@"facebook" controller:self animated:YES completion:^(MSUser *user, NSError *error) {
                [self refresh];
            }];
        }
```

* Replace `[self refresh]` in `viewDidLoad` with the following:

```
        [self loginAndGetData];
```

* Press  **Run** to start the app, and then log in. When you are logged in, you should be able to view the Todo list and make updates.
