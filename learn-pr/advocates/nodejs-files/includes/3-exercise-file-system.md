You can use Node.js to find and return information about files and folders.

Tailwind Traders has many physical stores all over the world. Each night, these stores create a file called *sales.json* that contains the total for all their sales for the previous day. These files are organized in folders named by store ID.

In this exercise, you write a Node.js program that can search for files called *sales.json* in a folder.


## Open project in development container

#### [Remote development (browser)](#tab/github-codespaces)

1. Start the process to create a new GitHub Codespace on the `main` branch of the [`MicrosoftDocs/node-essentials`](https://github.com/MicrosoftDocs/node-essentials) GitHub repository.

    > [!div class="nextstepaction"]
    > [Open this project in GitHub Codespaces](https://github.com/codespaces/new?azure-portal=true&hide_repo_select=true&ref=main&repo=278117471)

1. On the **Create codespace** page, review the codespace configuration settings and then select **Create new codespace**

    :::image type="content" source="../media/codespaces/codespace-configuration.png" alt-text="Screenshot of the confirmation screen before creating a new codespace.":::

1. Wait for the codespace to start. This startup process can take a few minutes.

1. Open a new terminal in the codespace.

    > [!TIP]
    > You can use the main menu to navigate to the **Terminal** menu option and then select the **New Terminal** option.
    >
    > :::image type="content" source="../media/codespaces/open-terminal-option.png" lightbox="../media/codespaces/open-terminal-option.png" alt-text="Screenshot of the codespaces menu option to open a new terminal.":::

1. Validate that Node.js is installed in your environment:

    ```bash
    node --version
    ```

    The dev container uses a Node.js LTS version such as `v20.5.1`. The exact version might be different.

1. The remaining exercises in this project take place in the context of this development container.

#### [Local development (Docker)](#tab/visual-studio-code)


1. Open **Visual Studio Code** in the context of an empty directory.
1. Ensure that you have the [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) installed in Visual Studio Code.
1. Open a new terminal in the editor.
    > [!TIP]
    > You can use the main menu to navigate to the **Terminal** menu option and then select the **New Terminal** option.
    >
    > :::image type="content" source="../media/codespaces/open-terminal-option.png" lightbox="../media/codespaces/open-terminal-option.png" alt-text="Screenshot of the menu option to open a new terminal.":::
1. Clone the [`MicrosoftDocs/node-essentials`](https://github.com/MicrosoftDocs/node-essentials) GitHub repository into the current directory.

    ```bash
    git clone https://github.com/MicrosoftDocs/node-essentials.git
    ```

1. Open the repository in code with the following commands in the terminal.

    ```bash
    cd node-essentials
    code .
    ```

1. Open the **Command Palette**, search for the **Dev Containers** commands, and then select **Dev Containers: Reopen in Container**.
    :::image type="content" source="../media/codespaces/reopen-container-command-palette.png" alt-text="Screenshot of the Command Palette option to reopen the current folder within the context of a development container.":::
    > [!TIP]
    > Visual Studio Code might automatically prompt you to reopen the existing folder within a development container. This is functionally equivalent to using the command palette to reopen the current workspace in a container.
    >
    > :::image type="content" source="../media/codespaces/reopen-container-toast.png" alt-text="Screenshot of a toast notification to reopen the current folder within the context of a development container.":::
1. Validate that Node.js is installed in your environment:

    ```bash
    node --version
    ```

    The dev container uses a Node.js LTS version such as `v20.5.1`. The exact version might be different.

1. The remaining exercises in this project take place in the context of this development container.

---


## Find the sales.json files

Your task is to find all the files in the *stores* folder.

Expand the *stores* folder and each of the numbered folders inside.

:::image type="content" source="../media/folder-structure.png" alt-text="Screenshot that shows the project folder structure.":::

### Include the fs module

1. In the `./nodejs-files` subfolder, create a *index.js* file to open it in the editor.

1. At the top of the file, add the following code in include the *fs* module is available to the code in the file.

   ```javascript
   const fs = require("fs").promises;
   ```

1. Next, create tnode he `main` method is the entry point for your code. The last line of code in this file invokes the `main` method.

   ```javascript
   const fs = require("fs").promises;

   async function main() {}

   main();
   ```

### Write a method to find the sales.json files

1. Create a new method called `findSalesFiles` that takes a `folderName` parameter.

   ```javascript
   async function findSalesFiles(folderName) {
     // FIND SALES FILES
   }
   ```

1. Add an array at the top, to hold the paths to all the sales files that the program finds.

   ```javascript
   async function findSalesFiles(folderName) {
     // this array will hold sales files as they are found
     let salesFiles = [];

     // FIND SALES FILES
   }
   ```

1. Create a method within this function called `findFiles`, which also takes a `folderName` parameter.

   ```javascript
   async function findSalesFiles(folderName) {
     // this array will hold sales files as they are found
     let salesFiles = [];

     async function findFiles(folderName) {
       // FIND SALES FILES
     }
   }
   ```

   This new method `findFiles` is created inside the main `findSalesFiles` method so that it can run as many times as necessary to find all the sales files and populate the `salesFiles` array. The `folderName` value is the path to the current folder.

1. Inside the `findFiles` method, read the `currentFolder` path with the `readdirsync` method.

   ```javascript
   async function findSalesFiles(folderName) {
     // this array will hold sales files as they are found
     let salesFiles = [];

     async function findFiles(folderName) {
       // read all the items in the current folder
       const items = await fs.readdir(folderName, { withFileTypes: true });

       // FIND SALES FILES
     }
   }
   ```

1. Add a block to loop over each item returned from the `readdirsync` method.

   ```javascript
   async function findSalesFiles(folderName) {
     // this array will hold sales files as they are found
     let salesFiles = [];

     async function findFiles(folderName) {
       // read all the items in the current folder
       const items = await fs.readdir(folderName, { withFileTypes: true });

       // iterate over each found item
       for (item of items) {
         // FIND SALES FILES
       }
     }
   }
   ```

1. Add an `if` statement to determine if the item is a file or a directory.

   ```javascript
   async function findSalesFiles(folderName) {
     // this array will hold sales files as they are found
     let salesFiles = [];

     async function findFiles(folderName) {
       // read all the items in the current folder
       const items = await fs.readdir(folderName, { withFileTypes: true });

       // iterate over each found item
       for (item of items) {
         if (item.isDirectory()) {
           // FIND SALES FILES IN THIS FOLDER
         } else {
           // FIND SALES FILES
         }
       }
     }
   }
   ```

1. If the item is a directory, call the `findFiles` method again, passing in the path to the item. If it's not, add a check to make sure the item name matches *sales.json*.

   ```javascript
   async function findSalesFiles(folderName) {
     // this array will hold sales files as they are found
     let salesFiles = [];

     async function findFiles(folderName) {
       // read all the items in the current folder
       const items = await fs.readdir(folderName, { withFileTypes: true });

       // iterate over each found item
       for (item of items) {
         if (item.isDirectory()) {
           // search this directory for files (this is recursion!)
           await findFiles(`${folderName}/${item.name}`);
         } else {
           // Make sure the discovered file is a sales.json file
           if (item.name === "sales.json") {
             // store the file path in the salesFiles array
             await salesFiles.push(`${folderName}/${item.name}`);
           }
         }
       }
     }
     await findFiles(folderName);
     return salesFiles;
   }
   ```

1. Call this new `findSaleFiles` function from the `main` method. Pass in the *stores* folder name as the location to search for files.

   ```javascript
   async function main() {
     const salesFiles = await findSalesFiles("stores");
     console.log(salesFiles);
   }
   ```

1. Press <kbd>Ctrl-S</kbd> to save the file, and press <kbd>Ctrl-Q</kbd> to close the editor.

## Run the program

1. Enter the following command into Cloud Shell to run the program.

   ```bash
   node index.js
   ```

1. The program should show the following output.

   ```bash
   [
    'stores/201/sales.json',
    'stores/202/sales.json',
    'stores/203/sales.json',
    'stores/204/sales.json',
   ]
   ```

Excellent! You've successfully written a command-line program that can traverse any directory and find all the *sales.json* files inside.

However, the way that the path to subfolders was constructed in this example is a little clumsy because it requires concatenating strings together. Also, you might run into issues on other operating systems (like Windows) that use different path separators. 

In the next section, you'll learn how to construct paths that work across operating systems by using the *path* module.

### Got stuck?

If you got stuck at any point in this exercise, here's the completed code. Remove everything in *index.js* and replace it with this solution.

```javascript
const fs = require("fs").promises;

async function findSalesFiles(folderName) {
  // this array will hold sales files as they are found
  let salesFiles = [];

  async function findFiles(folderName) {
    // read all the items in the current folder
    const items = await fs.readdir(folderName, { withFileTypes: true });

    // iterate over each found item
    for (item of items) {
      // if the item is a directory, it will need to be searched for files
      if (item.isDirectory()) {
        // search this directory for files (this is recursion!)
        await findFiles(`${folderName}/${item.name}`);
      } else {
        // Make sure the discovered file is a sales.json file
        if (item.name === "sales.json") {
          // store the file path in the salesFiles array
          await salesFiles.push(`${folderName}/${item.name}`);
        }
      }
    }
  }

  // find the sales files
  await findFiles(folderName);

  // return the array of found file paths
  return salesFiles;
}

async function main() {
  const salesFiles = await findSalesFiles("stores");
  console.log(salesFiles);
}

main();
```
