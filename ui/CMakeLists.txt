
set(RESOURCE_LIST
  STRIPBLANKS "account_cannot_remove.glade"
  STRIPBLANKS "account_list.glade"
  STRIPBLANKS "account_spinner.glade"
  STRIPBLANKS "certificate_warning_dialog.glade"
              "client-web-view.js"
              "client-web-view-allow-remote-images.js"
  STRIPBLANKS "composer-headerbar.ui"
  STRIPBLANKS "composer-link-popover.ui"
  STRIPBLANKS "composer-menus.ui"
  STRIPBLANKS "composer-widget.ui"
              "composer-web-view.css"
              "composer-web-view.js"
  STRIPBLANKS "conversation-email.ui"
  STRIPBLANKS "conversation-email-attachment-view.ui"
  STRIPBLANKS "conversation-email-menus.ui"
  STRIPBLANKS "conversation-message.ui"
  STRIPBLANKS "conversation-message-menus.ui"
  STRIPBLANKS "conversation-viewer.ui"
              "conversation-web-view.css"
              "conversation-web-view.js"
  STRIPBLANKS "edit_alternate_emails.glade"
  STRIPBLANKS "empty-placeholder.ui"
  STRIPBLANKS "find_bar.glade"
  STRIPBLANKS "folder-popover.ui"
  STRIPBLANKS "gtk/help-overlay.ui"
  STRIPBLANKS "gtk/menus.ui"
  STRIPBLANKS "login.glade"
  STRIPBLANKS "main-toolbar.ui"
  STRIPBLANKS "main-toolbar-menus.ui"
  STRIPBLANKS "main-window.ui"
  STRIPBLANKS "password-dialog.glade"
  STRIPBLANKS "preferences-dialog.ui"
  STRIPBLANKS "remove_confirm.glade"
  STRIPBLANKS "upgrade_dialog.glade"
  "geary.css"
)

compile_gresources(
  RESOURCES_C
  RESOURCES_XML
  TYPE EMBED_C
  SOURCE_DIR "${CMAKE_SOURCE_DIR}/ui"
  PREFIX "/org/gnome/Geary"
  RESOURCES ${RESOURCE_LIST}
)

add_custom_target(resource_c DEPENDS ${RESOURCES_C})

# Work around valac wanting the resource files to be in the same
# directory as the XML file.
add_custom_target(resource_copy ALL
    mkdir -p ${CMAKE_BINARY_DIR}/ui
  COMMAND
    cp ${RESOURCES_XML} ${CMAKE_BINARY_DIR}/ui
  COMMAND
    cp ${CMAKE_SOURCE_DIR}/ui/*.glade ${CMAKE_SOURCE_DIR}/ui/*.ui ${CMAKE_BINARY_DIR}/ui
  COMMAND
    cp ${CMAKE_SOURCE_DIR}/ui/*.css ${CMAKE_BINARY_DIR}/ui
)
add_dependencies(resource_copy resource_c)

# Export the file names so they can be used in the source build
set(RESOURCES_C ${RESOURCES_C} PARENT_SCOPE)
set(RESOURCES_XML "${CMAKE_BINARY_DIR}/ui/.gresource.xml" PARENT_SCOPE)
