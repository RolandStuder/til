Saw this in the fizzy codebase, apparently you can use `Current.with(account: Account.first) do `:

```ruby
    def attach_blob_as_rich_text_embed(container)
      Current.with(account: @account, session: sessions(:david)) do
        blob = ActiveStorage::Blob.create_and_upload! \
          io: file_fixture("moon.jpg").open,
          filename: "embed.jpg",
          content_type: "image/jpeg"

        attachment_html = ActionText::Attachment.from_attachable(blob).to_html
        if container.respond_to?(:description)
          container.update!(description: "<p>Description with image: #{attachment_html}</p>")
        else
          container.update!(body: "<p>Body with image: #{attachment_html}</p>")
        end

        blob.reload
      end
    end
```
