---
description: Built-in editor for Gunbot JSON config files and custom JavaScript strategies.
---

# Code editor

Edit Gunbot config files, view internal data and work on custom strategies right in the Gunbot interface.   


Unless you prefer working with config files or JavaScript code, you will never need this editor.  
If you do prefer config files, this editor allows you to configure everything you would normally do with an external text editor, and makes sure you don't save any invalid JSON to your configs.

![](../../.gitbook/assets/image%20%28128%29.png)

## Supported files

Currently the following file types are supported:

<table>
  <thead>
    <tr>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><code>config.js</code>
      </td>
      <td style="text-align:left">Main config file. Read and write support.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>autoconfig.json</code>
      </td>
      <td style="text-align:left">
        <p>Used to define AutoConfig jobs. Read and write support.</p>
        <p></p>
        <p>Editor has templates for all job and filter types, as autocomplete suggestions.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>pair state files</code>
      </td>
      <td style="text-align:left">Internal data. Read only.</td>
    </tr>
    <tr>
      <td style="text-align:left"><code>custom strategies</code>
      </td>
      <td style="text-align:left">
        <p>Custom JavaScript strategies, saved in the <code>/customStrategies</code> folder.
          <br
          />Read and write support.</p>
        <p></p>
        <p>Editor has autocomplete suggestions for all Gunbot specific methods and
          data.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><code>config backups</code>
      </td>
      <td style="text-align:left">
        <p>Backups of <code>config.js</code> and <code>autoconfig.json</code>. Read
          only.</p>
        <p>
          <br />Each time a config changed is done though the GUI, a config backup of
          the previous state is saved. You can view every past revision.</p>
      </td>
    </tr>
  </tbody>
</table>



## Code suggestions

The editor has autocomplete suggestions to assist in writing valid autoconfig jobs, and to have easy access to custom strategy methods and data point.

![Type any part of an autoconfig job or filter type to get suggestions](../../.gitbook/assets/image%20%28130%29.png)

![Get a clean job template after clicking a suggestion](../../.gitbook/assets/image%20%28131%29.png)

![Type any part of a custom strategy method or data point to view suggestions.](../../.gitbook/assets/image%20%28132%29.png)

